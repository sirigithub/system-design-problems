graph TB
    %% Define Control Plane components
    subgraph "Control Plane"
        CP[Kong Control Plane]
        KongDB[(Kong Config DB<br/>PostgreSQL)]
        Admin[Kong Admin API]
        
        %% Identity & Access Management subgraph
        subgraph "Identity & Access Management"
            IAM[IAM Service]
            IdentityDB[(Identity DB<br/>PostgreSQL/MySQL)]
            TM[Tenant Management]
            TeamM[Team Management]
            UM[User Management]
            
            %% IAM connections
            IAM --- IdentityDB
            TM --- IdentityDB
            TeamM --- IdentityDB
            UM --- IdentityDB
        end
        
        %% Configuration Management subgraph
        subgraph "Configuration Management"
            CM[Config Management]
            PM[Plugin Management]
            RM[Route Management]
            SM[Service Management]
            
            %% Config connections
            CM --- KongDB
            PM --- KongDB
            RM --- KongDB
            SM --- KongDB
        end
    end
    
    %% Define Data Plane components
    subgraph "Data Plane"
        DP1[Kong Data Plane 1]
        DP2[Kong Data Plane 2]
        DP3[Kong Data Plane 3]
    end
    
    %% Define Kubernetes Infrastructure
    subgraph "Kubernetes Infrastructure"
        K8s[Kubernetes API Server]
        CRD[Kong CRDs]
        Ingress[Ingress Controller]
        
        %% K8s connections
        K8s --- CRD
        K8s --- Ingress
    end
    
    %% Define Tenant Structure
    subgraph "Tenant Hierarchy Example"
        T1[Tenant 1]
        T2[Tenant 2]
        Team1[Team 1]
        Team2[Team 2]
        U1[User 1]
        U2[User 2]
        U3[User 3]
        
        %% Tenant hierarchy connections
        T1 --- Team1
        T1 --- Team2
        Team1 --- U1
        Team1 --- U2
        Team2 --- U3
    end
    
    %% Core relationships
    CP --- KongDB
    CP --- Admin
    CP --- IAM
    CP --- CM
    CP --- K8s
    
    %% Config sync connections
    CP ---|Config Sync| DP1
    CP ---|Config Sync| DP2
    CP ---|Config Sync| DP3
    
    %% Access flow
    U1 --> IAM
    IAM --> CP
    
    %% Traffic flow
    Client((Client)) --> DP1
    Client --> DP2
    Client --> DP3
    
    %% Styling
    style CP fill:#f9f,stroke:#333,stroke-width:2px
    style DP1 fill:#bbf,stroke:#333,stroke-width:2px
    style DP2 fill:#bbf,stroke:#333,stroke-width:2px
    style DP3 fill:#bbf,stroke:#333,stroke-width:2px
    style IdentityDB fill:#85C1E9,stroke:#333,stroke-width:2px
    style KongDB fill:#85C1E9,stroke:#333,stroke-width:2px