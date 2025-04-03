# jjhjnh

# Data Flow Diagram

```mermaid
graph TD
    CV[CCTV Camera] --> VF[Video Feed]
    VF --> FD[Face Detection]
    FD --> FR[Face Recognition]
    
    FR --> |Known Face| KF[Known Face Processing]
    FR --> |Unknown Face| UF[Unknown Face Processing]
    
    KF --> DB[(Database)]
    KF --> AS[Alert System]
    KF --> SI[Save Image]
    
    UF --> SI
    
    SI --> KFF[Known Faces Folder]
    SI --> UFF[Unknown Faces Folder]
    
    VF --> VR[Video Recording]
    VR --> RF[Recordings Folder]
    
    DB --> DU[Database Updates]
    DU --> LOC[Location Tracking]
    DU --> TS[Timestamp Updates]
```

# Entity Relationship Diagram

```mermaid
erDiagram
    USER {
        int id PK
        string name
        string location
        timestamp time
    }
    
    FACE_IMAGE {
        string filename PK
        string person_name FK
        timestamp capture_time
        string image_type
    }
    
    RECORDING {
        string filename PK
        timestamp start_time
        int duration
    }
    
    USER ||--o{ FACE_IMAGE : "has"
    RECORDING ||--|{ FACE_IMAGE : "contains"
    
```

System Components Description:

1. **User Management**
   - Primary key: ID (Auto-incrementing)
   - Stores user name, current location, and timestamp
   - Updated in real-time during face recognition

2. **Face Images**
   - Stored in separate folders for known/unknown faces
   - Filename includes person name and timestamp
   - Linked to user records for known faces

3. **Video Recordings**
   - Continuous recording in configurable duration segments
   - Stored with timestamp-based filenames
   - Contains multiple captured faces

4. **Real-time Processing**
   - Video capture from CCTV camera
   - Face detection and recognition
   - Alert system for recognized faces
   - Database updates for location tracking
