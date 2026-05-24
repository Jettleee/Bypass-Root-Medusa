# LAB 12 : Bypass de la Détection de Root Android avec Medusa

**Réalisé par :** Youssef Charaf  
**Établissement :** ENSA Marrakech  
**Environnement de travail :** Hôte Windows (avec Frida préinstallé) / Appareil Android (ou émulateur) rooté.

---

## 🎯 Objectif du Lab
Ce laboratoire vise à démontrer comment contourner les mécanismes de détection de root d'une application Android. L'approche s'appuie sur l'instrumentation dynamique pour intercepter et modifier le comportement des appels de l'application à l'exécution, en utilisant **Medusa** ainsi que des scripts **Frida** purs.

---

## 🛠️ Étape 1 : Préparation de l'environnement (Frida & ADB)

Étant donné que les outils Frida (`frida`, `frida-tools`) étaient déjà opérationnels sur la machine Windows hôte, la première étape a consisté à lier l'hôte à l'appareil Android via ADB et à déployer le serveur Frida.

1. **Vérification de la connexion ADB :**
   ```bash
   adb devices

```

*L'appareil est bien reconnu et autorisé.*

2. **Déploiement de `frida-server` :**
Identification de l'architecture du CPU (ex: `arm64-v8a`) et transfert du binaire correspondant :
```bash
adb shell getprop ro.product.cpu.abi
adb push frida-server /data/local/tmp/
adb shell chmod 755 /data/local/tmp/frida-server
adb shell "/data/local/tmp/frida-server -l 0.0.0.0"

```


3. **Validation de la communication :**
Listage des processus en cours d'exécution sur l'appareil pour confirmer le bon fonctionnement de l'instrumentation :
```bash
frida-ps -Uai

```



> <img width="922" height="192" alt="image" src="https://github.com/user-attachments/assets/f9b7e9ec-dbfb-4d88-9057-c6b6b7ec3e49" />


---

## 🐍 Étape 2 : Installation et Configuration de Medusa

Medusa est utilisé pour automatiser le processus de hook et simplifier le contournement des sécurités (anti-root, anti-frida, etc.).

1. **Clonage et installation des dépendances :**
```bash
git clone https://github.com/Ch0pin/medusa.git
cd Medusa
pip install -r requirements.txt

```


<img width="969" height="231" alt="image" src="https://github.com/user-attachments/assets/7be3afde-4394-485c-bb85-bf23a19bd5e7" />


2. **Vérification du fonctionnement de la CLI :**
```bash
python medusa.py --help

```



---

## 🚀 Étape 3 : Exécution du Bypass avec Medusa

L'application cible effectue plusieurs vérifications (lecture de `android.os.Build.TAGS`, présence de binaires comme `su` ou `busybox`, exécution de `Runtime.getRuntime().exec("su")`). Medusa permet de neutraliser ces vérifications dynamiquement.


1. **Lancement de l'application avec le module `root-bypass` injecté :**
```bash
   python medusa.py --usb --spawn sg.vantagepoint.uncrackable1 --module root-bypass --no-pause

```

*(Note : `com.example.rootcheck` a été remplacé par `sg.vantagepoint.uncrackable1`, le package réel de l'application analysée).*

2. **Observation :**
Les hooks sont installés avec succès. L'application, qui affichait initialement une alerte de compromission, considère désormais l'environnement comme sain (« Not rooted »).
> **Preuves :**
> <img width="326" height="644" alt="image" src="https://github.com/user-attachments/assets/2df325de-67dd-4750-9657-98d2be8dc492" />



> <img width="767" height="394" alt="image" src="https://github.com/user-attachments/assets/3047a14d-49e4-423e-ac5f-1d799af24905" />



> <img width="956" height="484" alt="image" src="https://github.com/user-attachments/assets/e4703f55-0b36-40d7-a0d5-43b90770ab6f" />


<img width="300" height="635" alt="image" src="https://github.com/user-attachments/assets/21a9de26-3395-4f0f-a5ab-3ecd0e2ae33e" />




---

## 🔧 Alternative : Plan B avec Frida pur (Scripts custom)

Afin d'approfondir la compréhension des mécanismes de hook, le bypass a également été réalisé en écrivant des scripts Frida personnalisés, ciblant directement les méthodes Java et natives.

1. **Bypass côté Java (`bypass_root.js`) :**
Ce script redéfinit `Build.TAGS`, falsifie le retour de `RootBeer.isRooted`, intercepte `File.exists` pour les chemins suspects (`/system/xbin/su`), et bloque les commandes envoyées via `Runtime.exec`.
2. **Bypass côté Natif (`bypass_native.js`) :**
Mise en place d'intercepteurs sur les appels système de bas niveau (`open`, `openat`, `access`, `stat`) pour empêcher la détection de fichiers liés au rootage depuis les librairies C/C++.
3. **Exécution combinée :**
```bash
frida -U -f com.example.rootcheck -l bypass_root.js -l bypass_native.js --no-pause

```


```

```
