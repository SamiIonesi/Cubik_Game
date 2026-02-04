# Cubik Game
**Aplicație de Realitate Virtuală pentru dezvoltarea abilităților cognitive și motrice.**

![Unity](https://img.shields.io/badge/Unity-2022.3+-black?style=flat&logo=unity)
![Platform](https://img.shields.io/badge/Platform-Meta_Quest_2-blue?style=flat&logo=oculus)
![Status](https://img.shields.io/badge/Status-Finished-green)

## 1. Introducere
**Cubik Game** este o aplicație VR terapeutică dezvoltată pentru **Meta Quest 2**, destinată persoanelor cu dizabilități cognitive sau motorii. Scopul principal este crearea unui mediu controlat, lipsit de distrageri, unde utilizatorii pot exersa coordonarea mână-ochi, recunoașterea formelor și atenția distributivă.

Proiectul pune accent pe accesibilitate și **întărirea pozitivă**, oferind un flux de joc ghidat pas cu pas prin instrucțiuni audio și vizuale clare.

---

## 2. Descrierea Jocului
Jocul plasează utilizatorul într-o cameră virtuală liniștită, așezat la o masă de lucru. Sarcina principală este identificarea, apucarea și potrivirea formelor geometrice în soclurile corespunzătoare.
![WhatsApp Image 2026-02-03 at 20 41 56](https://github.com/user-attachments/assets/fa2a4865-b212-4093-8ee6-d9f4798d84ff)

### Funcționalități Cheie:
* **Gameplay Infinit & Ciclic**: Nivelurile se regenerează automat, permițând sesiuni de antrenament continue fără ecrane de "Game Over".
* **Coordonare Bilaterală**: Formele sunt generate aleatoriu în stânga sau în dreapta utilizatorului, încurajând folosirea ambelor mâini și trecerea liniei mediene a corpului.
* **Feedback Multimodal**:
    * *Vizual*: Schimbarea culorii soclului (Verde/Roșu) și efecte de particule.
    * *Auditiv*: Instrucțiuni vocale (Text-to-Speech) și efecte sonore (SFX) pentru succes/eroare.
    * *Text*: Instrucțiuni afișate pe un monitor virtual în fața utilizatorului.

---

## 3. Platforme și Tool-uri Folosite
Proiectul a fost realizat folosind un stack tehnologic modern pentru dezvoltarea VR:

| Categorie | Tool / Tehnologie | Detalii |
| :--- | :--- | :--- |
| **Motor Grafic** | **Unity 2022.3 LTS** | Universal Render Pipeline (URP) |
| **VR Framework** | **XR Interaction Toolkit** | Gestionarea Grab, Ray Interactors și Sockets |
| **Hardware** | **Meta Quest 2** | Platformă target Android |
| **Limbaj** | **C#** | Scripting pentru logica jocului |
| **UI** | **TextMeshPro** | Afișare text clară în VR |
| **Audio** | **Audacity / TTS** | Procesarea instrucțiunilor vocale |

---

## 4. Arhitectura Sistemului (Diagrama Bloc)
Sistemul este construit pe o arhitectură centralizată, unde scriptul **GameManager** coordonează fluxul de date între jucător (Input) și sistemele de feedback (Output).

<img width="1182" height="692" alt="image" src="https://github.com/user-attachments/assets/8c895678-35be-4a6c-a215-07f7b4ab4867" />


### Componente Principale:

#### **1. Game Manager (Central Logic Control)**
Acesta este pilonul central al aplicației, implementat ca un Singleton pentru a permite accesul global din orice alt script fără referințe complexe.

Gestionarea Stărilor (State Machine): Controlează fluxul jocului prin stări distincte: Inițializare Scenă -> Generare Nivel -> Așteptare Input Jucător -> Validare -> Feedback -> Resetare Nivel.

Corutine Audio (Audio Coroutines): Utilizează IEnumerator pentru a gestiona secvențialitatea instrucțiunilor vocale. Acest lucru previne suprapunerea sunetelor ("clipping") asigurând că instrucțiunea "Ia forma..." se termină complet înainte ca numele formei să fie pronunțat.

Baza de Date a Formelor (Scriptable Logic): Deține o listă de obiecte de tip ShapePair, care leagă prefab-ul vizual (forma reală), "fantoma" (socket-ul) și clipurile audio specifice fiecărei forme.

🎮 2. XR Interaction System (Interacțiune Fizică)
Sistemul bazat pe Unity XR Interaction Toolkit gestionează puntea dintre acțiunile fizice ale utilizatorului și lumea virtuală.

VR Player Rig: Configurează camera pentru a simula înălțimea unui utilizator așezat (Seated Mode), esențial pentru accesibilitate. Controllerele sunt mapate pentru a urmări mișcarea mâinilor în timp real.

XR Grab Interactables: Obiectele geometrice au configurate puncte de prindere (Attach Points) precise pentru a preveni "intrarea" mâinii virtuale în obiect.

Smart Sockets (Validare Logică):

Fiecare zonă de pe masă este un XRSocketInteractor modificat.

Acestea nu doar atrag obiectele, ci interoghează activ Tag-ul obiectului introdus.

Mecanism Anti-Flicker: Dacă utilizatorul apropie o piesă greșită, socket-ul o respinge fizic și se dezactivează temporar (Cooldown) pentru a evita declanșarea repetată a sunetului de eroare.

📢 3. Feedback System (Multimodal Output)
Sistemul este conceput să ofere răspunsuri simultane pe trei canale senzoriale pentru a maximiza înțelegerea sarcinii.

Visual Feedback (SocketColorFeedback.cs):

Script dedicat atașat fiecărui socket care schimbă materialul "fantomei" în timp real: Gri (Neutru), Verde (Corect - la validare), Roșu (Greșit - la încercare eșuată).

La plasarea corectă, fizica obiectului este dezactivată (isKinematic = true), iar obiectul se "lipește" vizual de socket.

Audio Feedback System:

Layering Audio: Utilizează două surse audio (AudioSource) distincte. Sursa de Muzică rulează pe un canal secundar la volum redus (Loop), în timp ce sursa de Voce/SFX are prioritate maximă (Priority High) și volum 100%.

UI Feedback (Monitor Virtual):

Un Canvas World-Space plasat ergonomic în fața utilizatorului.

Textul este actualizat dinamic prin GameManager pentru a reflecta exact instrucțiunea vocală curentă, oferind suport pentru persoanele cu deficiențe de auz.

---

## 5. Fluxul de Execuție (Workflow)
Logica jocului urmează un algoritm liniar cu bucle de validare pentru a asigura corectitudinea acțiunilor utilizatorului.

<img width="625" height="746" alt="image" src="https://github.com/user-attachments/assets/96986201-f6b9-47a5-aaa0-6e29c308f509" />

### Descrierea Pașilor:
1.  **Inițializare**: Se încarcă scena, pornește muzica de fundal (volum redus) și mesajul de bun venit.
2.  **Spawn & Instrucțiune**:
    * GameManager alege o formă aleatorie și o poziție (Stânga/Dreapta).
    * Se declanșează secvența audio: *"Ia forma [X] din [Stânga]..."*.
3.  **Acțiune & Validare**:
    * Jucătorul pune piesa în soclu.
    * Scriptul `SocketColorFeedback` verifică Tag-ul obiectului.
4.  **Ramificare (Decision)**:
    * **Corect**: Piesa se blochează (Kinematic), devine verde -> Feedback Pozitiv.
    * **Greșit**: Piesa este respinsă, devine roșie -> Feedback Corectiv -> Cooldown 5 secunde.

---

## 6. Implementare Tehnică (Highlights)

### Gestionarea Audio (Evitarea Suprapunerilor)
Pentru a preveni haosul auditiv (muzică + voce + efecte simultan), am folosit **Corutine** (`IEnumerator`) pentru a aștepta terminarea unui clip audio înainte de a începe următorul.

```csharp
// Exemplu din GameManager.cs
System.Collections.IEnumerator RedaInstructiuneVocala(ShapePair pair)
{
    audioSursa.PlayOneShot(voceIaForma);
    yield return new WaitForSeconds(voceIaForma.length); // Așteaptă terminarea
    audioSursa.PlayOneShot(pair.voceNumeForma);
}
```

### Validarea Soclurilor (Socket Logic)
Am implementat un sistem de cooldown pentru a preveni erorile multiple ("flickering") atunci când utilizatorul ține piesa greșită deasupra soclului.

```csharp
// Exemplu din SocketColorFeedback.cs
private System.Collections.IEnumerator ReenableSocketAfterDelay()
{
    yield return new WaitForSeconds(5.0f); // Pauză de 5 secunde după eroare
    if (!isLocked) socket.socketActive = true;
}
```

## 7. Provocări și Soluții
Pe parcursul dezvoltării am întâmpinat mai multe obstacole tehnice:

### Limitări Hardware (GPU Laptop):

Problemă: Laptopul de dezvoltare nu a putut rula fluid prin Oculus Link.

Soluție: Am adoptat fluxul de lucru Build and Run, testând direct pe hardware-ul nativ al căștii Quest 2 pentru acuratețe maximă.

### Interacțiunea cu Mâinile:

Problemă: Dificultăți în îmbinarea unui model 3D de corp uman cu sistemul de tracking.

Soluție: Am adaptat și personalizat prefab-ul standard XR Hands din Unity, optimizându-l pentru precizie în apucarea obiectelor mici.

### Gestionarea Audio:

Problemă: Fișierele audio mari creau latență, iar volumele se suprapuneau.

Soluție: Am implementat două surse audio distincte (una pentru fundal la 20% volum, una pentru voce la 100%) și am activat opțiunea "Preload Audio Data".

## 8. Concluzii și Direcții Viitoare
Aplicația Cubik Game reprezintă o soluție funcțională și stabilă pentru reabilitarea prin VR. Testele au demonstrat că feedback-ul imediat și ghidajul vocal reduc anxietatea utilizatorului și cresc rata de succes a sarcinilor.

Îmbunătățiri viitoare:

- Implementare corp pacient + maini

- Hand Tracking mai stabil

- Salvarea progresului pacientului într-o bază de date + rapoarte.

- Niveluri de dificultate (posibil).

- Interactiunea cu obiectele mai buna.

