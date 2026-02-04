# Cubik Game - VR Cognitive Rehabilitation
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

[Cubik_Game_Diagram.pdf](https://github.com/user-attachments/files/25075866/Cubik_Game_Diagram.pdf)

*(Notă: Urcă imaginea diagramei generate în folderul Media al repo-ului)*

### Componente Principale:
1.  **GameManager (Singleton)**: "Creierul" aplicației. Gestionează stările jocului, scorul și cronometrarea evenimentelor.
2.  **XR Interaction System**:
    * **VR Player**: Camera și Controllerele.
    * **Socket Interactors**: Logica de validare a formelor pe masă.
3.  **Feedback System**: Sincronizează mesajele de pe monitor cu fișierele audio și efectele vizuale.

---

## 5. Fluxul de Execuție (Workflow)
Logica jocului urmează un algoritm liniar cu bucle de validare pentru a asigura corectitudinea acțiunilor utilizatorului.

[Cubik_Game_Flow.pdf](https://github.com/user-attachments/files/25075872/Cubik_Game_Flow.pdf)

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

Iată documentația completă, formatată profesional în Markdown, gata de a fi copiată direct în fișierul README.md de pe GitHub.

Am inclus toate secțiunile discutate (Introducere, Arhitectură, Workflow, Provocări) și am lăsat locurile marcate pentru imaginile pe care le-am generat anterior.

Markdown
# Cubik Game - VR Cognitive Rehabilitation
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

![Diagrama Bloc](Media/diagrama_bloc.png)
*(Notă: Urcă imaginea diagramei generate în folderul Media al repo-ului)*

### Componente Principale:
1.  **GameManager (Singleton)**: "Creierul" aplicației. Gestionează stările jocului, scorul și cronometrarea evenimentelor.
2.  **XR Interaction System**:
    * **VR Player**: Camera și Controllerele.
    * **Socket Interactors**: Logica de validare a formelor pe masă.
3.  **Feedback System**: Sincronizează mesajele de pe monitor cu fișierele audio și efectele vizuale.

---

## 5. Fluxul de Execuție (Workflow)
Logica jocului urmează un algoritm liniar cu bucle de validare pentru a asigura corectitudinea acțiunilor utilizatorului.

![Workflow Diagram](Media/workflow_landscape.png)
*(Notă: Urcă imaginea workflow-ului landscape în folderul Media al repo-ului)*

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
Validarea Soclurilor (Socket Logic)
Am implementat un sistem de cooldown pentru a preveni erorile multiple ("flickering") atunci când utilizatorul ține piesa greșită deasupra soclului.

C#
// Exemplu din SocketColorFeedback.cs
private System.Collections.IEnumerator ReenableSocketAfterDelay()
{
    yield return new WaitForSeconds(5.0f); // Pauză de 5 secunde după eroare
    if (!isLocked) socket.socketActive = true;
}
7. Provocări și Soluții
Pe parcursul dezvoltării am întâmpinat mai multe obstacole tehnice:

Limitări Hardware (GPU Laptop):

Problemă: Laptopul de dezvoltare nu a putut rula fluid prin Oculus Link.

Soluție: Am adoptat fluxul de lucru Build and Run, testând direct pe hardware-ul nativ al căștii Quest 2 pentru acuratețe maximă.

Interacțiunea cu Mâinile:

Problemă: Dificultăți în îmbinarea unui model 3D de corp uman cu sistemul de tracking.

Soluție: Am adaptat și personalizat prefab-ul standard XR Hands din Unity, optimizându-l pentru precizie în apucarea obiectelor mici.

Gestionarea Audio:

Problemă: Fișierele audio mari creau latență, iar volumele se suprapuneau.

Soluție: Am implementat două surse audio distincte (una pentru fundal la 20% volum, una pentru voce la 100%) și am activat opțiunea "Preload Audio Data".

8. Concluzii și Direcții Viitoare
Aplicația Cubik Game reprezintă o soluție funcțională și stabilă pentru reabilitarea prin VR. Testele au demonstrat că feedback-ul imediat și ghidajul vocal reduc anxietatea utilizatorului și cresc rata de succes a sarcinilor.

Îmbunătățiri viitoare:

Implementarea Hand Tracking nativ (renunțarea la controllere).

Adăugarea unui sistem de Analiză a Datelor pentru a salva progresul pacienților (timp de reacție, număr de erori) într-o bază de date pentru medici.
