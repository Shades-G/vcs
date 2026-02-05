graph
    %% Project Approval Workflow
    Start((Start)) --> S_Apply[Student: Apply for Project]
    S_Apply --> DB_Pending[Database: Status = PENDING]
    DB_Pending --> F_Review{Faculty Review}
    
    F_Review -- Approve --> DB_Approved[Database: Status = APPROVED]
    F_Review -- Reject --> DB_Rejected[Database: Status = REJECTED]
    F_Review -- Clarify --> DB_Changes[Request Changes]
    
    DB_Approved --> End((Project Active))
    DB_Rejected --> End
    DB_Changes --> S_Apply
