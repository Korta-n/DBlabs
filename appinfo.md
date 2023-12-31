# Comprehensive description of object domain, associated database, and app functionality

---

## Functionality

All users can be devided into two groups:
1. Candidates
2. Clients


#### Client can
1. Manage their jobs (dates, slaries, all fields)
2. Manage their profile info
3. Manage Placements to look or change info
4. View all candidates and possible placements for his jobs
5. Receive paying bills

#### Candidate
1. Manage their profile
2. Manage their placements or applications to jobs
3. Manage their job status and request additional info

#### Admin can
1. Manage (delete, update info, etc.) of jobs | clients | candidates | placements
2. Add clients to the platform | collect jobs info from clients
3. Look through activity logs and change system behaviour.

## Database

### Diagram
[Database visualization](https://drawsql.app/teams/dblabs/diagrams/lab123)

### Description

### Clients Table

- **ClientID**: SERIAL (Auto-incrementing integer)
- **Client Name**: VARCHAR(255)
- **Contact Information (Phone, Email)**: VARCHAR(255)
- **Billing Information**: TEXT or VARCHAR(255) (depending on complexity)
- **Contract Details**: TEXT
- **Notes**: TEXT

**Relationships**:
- One-to-Many with Jobs Table: Each client can have multiple job openings.

### Candidates Table

- **CandidateID**: SERIAL
- **First Name**: VARCHAR(50)
- **Last Name**: VARCHAR(50)
- **Contact Information (Phone, Email)**: VARCHAR(255)
- **Resume/CV**: TEXT or VARCHAR(255) (store a file path or URL)
- **Skills and Qualifications**: TEXT
- **Availability**: DATE or TIMESTAMP
- **Notes**: TEXT

**Relationships**:
- Many-to-Many with Jobs Table through Placements Table: Candidates can apply to multiple jobs, and each job can have multiple candidates in consideration.

### Jobs Table

- **JobID**: SERIAL
- **Job Title**: VARCHAR(255)
- **Client (Foreign Key to Clients)**: INTEGER (Reference to ClientID)
- **Job Description**: TEXT
- **Required Skills**: TEXT or JSON (for structured skill requirements)
- **Salary/Rate**: DECIMAL(10, 2) (for monetary values)
- **Location**: VARCHAR(255)
- **Start Date**: DATE or TIMESTAMP
- **End Date**: DATE or TIMESTAMP
- **Status (Open, Closed, In Progress)**: VARCHAR(20)

**Relationships**:
- One-to-Many with Placements Table: Each job can have multiple placements.
- One-to-Many with Clients Table: Each job is associated with a client.

### Placements Table

- **PlacementID**: SERIAL
- **Candidate (Foreign Key to Candidates)**: INTEGER (Reference to CandidateID)
- **Job (Foreign Key to Jobs)**: INTEGER (Reference to JobID)
- **Start Date**: DATE or TIMESTAMP
- **End Date**: DATE or TIMESTAMP
- **Billing Rate**: DECIMAL(10, 2)
- **Placement Status (Active, Completed, Canceled)**: VARCHAR(20)

**Relationships**:
- Many-to-Many with Candidates Table and Jobs Table: This table establishes a many-to-many relationship between candidates and jobs.
- One-to-Many with Users Table: Track which staff member is responsible for each placement.

### User Table (for User Management and Authentication)

- **UserID**: SERIAL
- **Username**: VARCHAR(50)
- **Password (hashed and salted)**: VARCHAR(255)
- **Email**: VARCHAR(255)
- **RoleID (Foreign Key to Roles)**: INTEGER

**Relationships**:
- Many-to-One with Roles Table: Each user is associated with a role that determines their permissions within the system.

### Roles Table

- **RoleID**: SERIAL
- **Role Name**: VARCHAR(50)
- **Permissions**: JSONB (store permissions as a JSON object)

**Relationships**:
- One-to-Many with Users Table: Each role can be associated with multiple users.

### User Activity Log Table

- **LogID**: SERIAL
- **UserID (Foreign Key to Users)**: INTEGER
- **Timestamp**: TIMESTAMP
- **Action**: VARCHAR(255)
- **Details**: TEXT or JSONB (store additional details as needed)

**Relationships**:
- Many-to-One with Users Table: Each log entry records the user responsible for the action.
