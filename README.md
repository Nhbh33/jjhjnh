# Face Recognition CCTV System Architecture

```mermaid
graph TD
    subgraph GUI[Face Entry System GUI]
        FE[FaceEntrySystem]
        UC[Upload/Capture Image]
        DB[(SQLite DB)]
        TV[CCTV Toggle]
    end

    subgraph FR[Face Recognition Core]
        SFR[SimpleFacerec]
        FD[Face Detection]
        FE1[Face Encoding]
        FM[Face Matching]
    end

    subgraph CCTV[CCTV System]
        VC[Video Capture]
        REC[Video Recording]
        AL[Alert System]
        FS[Face Storage]
    end

    %% Data flow relationships
    UC -->|Images| FR
    FR -->|Face Data| DB
    TV -->|Controls| CCTV
    VC -->|Frame| FR
    FR -->|Results| CCTV
    CCTV -->|Location Updates| DB

    %% File system interactions
    subgraph FS[File Storage]
        IMG[/images/]
        KF[/known_faces/]
        UF[/unknown_faces/]
        VR[/cctv_recordings/]
    end

    UC -->|Upload| IMG
    FS -->|Load| FR
    CCTV -->|Save| KF
    CCTV -->|Save| UF
    CCTV -->|Record| VR

    %% Component details
    subgraph Components
        direction LR
        FD -->|Processes| FE1
        FE1 -->|Compares| FM
    end
```

# Key Components

## 1. Face Entry System (GUI)
- Manages user registration
- Handles image capture/upload
- Controls CCTV system
- Displays recognition results

## 2. Face Recognition Core
- Loads and manages face encodings
- Performs face detection
- Matches faces against database
- Returns recognition results

## 3. CCTV System
- Captures video feed
- Records surveillance footage
- Triggers alerts for recognized faces
- Stores face images and recordings

## 4. Storage System
- SQLite database for user data
- Image directory for training data
- Separate folders for known/unknown faces
- CCTV recordings archive

## 5. Data Flow
- Images → Face Recognition → Database
- Video Feed → Face Detection → Recognition → Alert
- Recognition Results → Database Updates
- Face Images → File Storage
