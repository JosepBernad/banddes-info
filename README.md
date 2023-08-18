# Banddes

This is a model specification for a multi-tenant SaaS platform tailored to choirs, bands, and orchestras. The platform allows ensembles to manage their sheet music libraries, users (musicians), rehearsals, and performances. The model covers entities such as ensemble, user, instrument, piece, file, creator, event, and with their respective attributes and relationships.

## Model

```yaml
Ensamble:
  id: integer (begins from 3000000)
  name: string
  ensembleType: EnsembleType
  accountCode: string
  website: string
  contactName: string
  contactEmail: string
  contactPhone: string

EnsembleType:
  enum(CHOIR, BAND, ORCHESTRA, BIG_BAND, JAZZ_BAND)

User:
  id: integer (begins from 1000)
  email: string
  password: string
  firstName: string
  lastName: string
  phoneNumber: string
  dateOfBirth: date
  userRole: UserRole
  instruments: array(Instrument)
  ensambles: array(Ensable)

UserRole:
  enum(GOD, SYSTEM, ADMIN, USER, GUEST)

Instrument:
  id: integer (begins from 1000)
  name: string
  ensamble: Ensamble

Piece:
  id: integer (begins from 1000)
  title: string
  genre: string
  duration: integer
  files: array(File)
  creators: array(Creator)
  ensamble: Ensable

File:
  id: integer (begins from 1000)
  name: string
  fileType: FileType
  extension: string
  source: string
  
FileType:
  enum(SCORE, SCORE_EDITABLE, MIDI, AUDIO)

Creator:
  id: integer (begins from 1000)
  firstName: string
  lastName: string
  creatorRole: CreatorRole

CreatorRole:
  enum(COMPOSER, SONGWRITER, ARRANGER, BAND, PRODUCER, LYRICIST)

Event:
  id: integer (begins from 1000)
  eventType: PerformanceType
  title: string
  startDate: time
  endDate: time
  location: string
  pieces: array(Piece)
  ensamble: Ensable
  
EventType:
  enum(REHERSAL, CONCERT, PARADE)
```

## Screens

### 1. Login

### 2. CRUD `Ensable`

UserRoles: \[god, system\] 

### 3. CRUD `User`

UserRoles: \[god, system, admin, user (only Read)\]

### 4. CRUD `Piece`

UserRoles: \[god, system, admin, user (only Read)\]

### 5. CRUD `Event`

UserRoles: \[god, system, admin, user (only Read)\]

### 6. Settings

UserRoles: \[god, system, admin, user\] 

## Use cases

### Use Case 1: Register a new ensemble

**Actors:** system

**Preconditions:**
- The user type system has access to the registration page.

**Steps:**
1. The user type system fills in the registration form, providing the ensemble's name, type, contact details, and other required information.
2. The system validates the input and creates a new `Ensemble` instance.
3. The user type system is redirected to the ensemble dashboard.

**Postconditions:**
- A new Ensemble is created and stored in the database.

### Use Case 2: Add a new user to an ensemble

**Actors:** admin

**Preconditions:**
- The ensemble admin is logged in.
- The ensemble admin has access to the user management section.

**Steps:**
1. The ensemble admin fills in the user registration form, providing the user's email, password, name, phone number, date of birth, user role, and associated instruments.
2. The system validates the input and creates a new `User` instance.
3. The new user is added to the ensemble's list of users.

**Postconditions:**
- A new user is created and associated with the ensemble.

### Use Case 3: Add a new piece of music to the ensemble's library

**Actors:** admin, user

**Preconditions:**
- The actor is logged in and has access to the ensemble's music library management section.

**Steps:**
1. The actor fills in the piece creation form, providing the title, genre, duration, file, and creator details.
2. The system validates the input and creates a new `Piece` instance.
3. The new piece is added to the ensemble's music library.

**Postconditions:**
- A new piece of music is created and associated with the ensemble.

### Use Case 4: Schedule a event for the ensemble

**Actors:** admin, user

**Preconditions:**
- The actor is logged in and has access to the ensemble's event management section.

**Steps:**
1. The actor fills in the event creation form, providing the title, date, start time, end time, location, and associated pieces and users.
2. The system validates the input and creates a new `Event` instance.
3. The new rehearsal is added to the ensemble's list of rehearsals.

**Postconditions:**
- A new event is created and associated with the ensemble.

### Use Case 5: Update user details

**Actors:** admin, user

**Preconditions:**
- The actor is logged in and has access to the user management section.

**Steps:**
1. The actor navigates to the user's profile and clicks the "Edit" button.
2. The actor modifies the user's details, such as name, email, phone number, date of birth, user role, or associated instruments.
3. The system validates the input and updates the `User` instance.

**Postconditions:**
- The user's details are updated.

### Use Case 6: Update the list of pieces in a event

**Actors:** admin, user

**Preconditions:**
- The actor is logged in and has access to the ensemble's event management section.
- The event exists in the system.

**Steps:**
1. The actor navigates to the event's details page and clicks the "Edit" button.
2. The actor modifies the list of associated pieces, adding or removing pieces as needed.
3. The system validates the input and updates the `Event` instance with the new list of pieces.

**Postconditions:**
- The list of pieces associated with the event is updated.
