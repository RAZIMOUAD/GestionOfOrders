
# GestionOfOrders

Un projet Java pour automatiser la gestion des commandes à partir de fichiers JSON. Il inclut la validation des données, la persistance dans une base de données MySQL, et la gestion des erreurs.

---

## **Fonctionnalités**

- Gestion des répertoires :
  - **`data/INPUT`** : Contient les fichiers JSON à traiter.
  - **`data/OUTPUT`** : Contient les fichiers JSON valides après traitement.
  - **`data/ERROR`** : Contient les fichiers JSON invalides après traitement.
- Lecture et conversion des fichiers JSON en objets `Order` et `Customer`.
- Validation des données :
  - Vérification que le `customer_id` existe dans la base de données.
  - Rejet des commandes avec un montant non positif.
- Persistance des données valides dans une base de données MySQL.
- Gestion automatique des fichiers JSON :
  - Déplacement des fichiers valides vers `data/OUTPUT`.
  - Déplacement des fichiers invalides vers `data/ERROR`.
- Journalisation détaillée des actions dans la console.

---

## **Prérequis**

- **Java** : Version 11 ou supérieure.
- **Maven** : Gestionnaire de dépendances.
- **MySQL** : Base de données relationnelle.

---

## **Installation**

1. Clonez le projet :
   ```bash
   git clone https://github.com/RAZIMOUAD/GestionOfOrders.git
   cd GestionOfOrders
   ```

2. Installez les dépendances Maven :
   ```bash
   mvn clean install
   ```

3. Configurez votre environnement MySQL :
   - Créez une base de données nommée `GestionOrders` :
     ```sql
     CREATE DATABASE GestionOrders;
     USE GestionOrders;
     ```
   - Créez les tables nécessaires :
     ```sql
     CREATE TABLE customers (
         id INT PRIMARY KEY,
         name VARCHAR(255),
         email VARCHAR(255),
         phone VARCHAR(20)
     );

     CREATE TABLE orders (
         id INT PRIMARY KEY,
         date TIMESTAMP,
         amount DOUBLE,
         customer_id INT,
         FOREIGN KEY (customer_id) REFERENCES customers(id)
     );
     ```

4. Configurez les identifiants de connexion MySQL dans `DatabaseConnection.java` :
   ```java
   private static final String URL = "jdbc:mysql://localhost:3306/GestionOrders";
   private static final String USER = "votre_utilisateur";
   private static final String PASSWORD = "votre_mot_de_passe";
   ```

---

## **Utilisation**

1. Ajoutez des fichiers JSON dans le dossier **`data/INPUT`**. Exemple de fichier valide :
   ```json
   {
       "id": 1,
       "date": "2024-06-15T12:30:00",
       "amount": 200.50,
       "customer_id": 101
   }
   ```

2. Lancez le programme :
   ```bash
   mvn exec:java -Dexec.mainClass="org.example.Main"
   ```

3. Résultats attendus :
   - Les fichiers JSON valides sont déplacés vers **`data/OUTPUT`** et insérés dans la base de données.
   - Les fichiers JSON invalides sont déplacés vers **`data/ERROR`**.

---

## **Structure du Projet**

```plaintext
GestionOfOrders/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   ├── org.example/
│   │   │   │   ├── Main.java                # Point d'entrée du programme
│   │   │   │   ├── models/
│   │   │   │   │   ├── Order.java           # Classe représentant une commande
│   │   │   │   │   ├── Customer.java        # Classe représentant un client
│   │   │   │   ├── utils/
│   │   │   │   │   ├── DirectoryManager.java # Gestion des répertoires
│   │   │   │   │   ├── JsonReader.java      # Lecture et validation des fichiers JSON
│   │   │   │   │   ├── OrderValidator.java  # Validation et persistance des commandes
│   │   ├── resources/
│   ├── test/                                # Tests unitaires
├── data/
│   ├── INPUT/                               # Fichiers JSON à traiter
│   ├── OUTPUT/                              # Fichiers JSON validés
│   ├── ERROR/                               # Fichiers JSON invalides
├── pom.xml                                  # Fichier de configuration Maven
```

---

## **Exemple d'Exécution**

### **Cas 1 : Fichiers JSON valides**
- Fichiers valides présents dans `data/INPUT`.
- Résultats en console :
  ```
  Initialisation du projet...
  Fichier traité avec succès : order1.json
  Fichier déplacé vers OUTPUT : order1.json
  Commandes valides traitées :
  Commande ID : 1, Montant : 200.50
  Processus terminé !
  ```

### **Cas 2 : Dossier INPUT vide**
- Aucun fichier JSON dans `data/INPUT`.
- Résultats en console :
  ```
  Initialisation du projet...
  Le répertoire INPUT est vide. Aucun fichier à traiter.
  Processus terminé !
  ```

### **Cas 3 : Fichiers JSON invalides**
- Fichiers JSON avec des erreurs dans `data/INPUT`.
- Résultats en console :
  ```
  Initialisation du projet...
  Erreur de lecture JSON pour le fichier : order4.json
  Le montant doit être positif
  Fichier déplacé vers ERROR : order4.json
  Processus terminé !
  ```

---

## **Améliorations Futures**

- Ajouter un fichier log pour enregistrer les erreurs et les actions.
- Implémenter une interface utilisateur graphique avec JavaFX.
- Automatiser le traitement avec une tâche planifiée (par exemple, un cron job).

---

## **Auteurs**

- **RAZI MOUAD** : Étudiant en génie Informatique.

---

## **Licence**

Ce projet est sous licence MIT. Consultez le fichier [LICENSE](LICENSE) pour plus de détails.
