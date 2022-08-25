# StickyNotebooks

StickyNotebooks are a new way to organize your thoughts to help you collect text, gather your research, write brainstorms, and be prepared with the subset of information you need. Picture Evernote, but with additional drag-and-drop views that help you pull together subsets of your notes for simultaneous viewing. Picture Mac Stickies, but with tabbable views that help you deep-dive on topics of your interest.

Project started 8/24/22.

StickyNotebooks will be a React / Express.js / MongoDB project.

## Current project status:

- Still in the planning stage.

## App Feature Goals:

- On the homepage you can see a searchable, sortable list of your StickyNotebook previews. You click on one and it takes you to that StickyNotebook page.
- The StickyNotebook page is a tabbed interface of text blocks. It's easy to create new tabs inside of a single StickyNotebook that collect related information (the "Notebook" view of a StickyNotebook).
- You can create "Views" that place certain StickyNotebooks side by side (the "Sticky" view of a StickyNotebook). You can adjust the size and location of each StickyNotebook inside the view. These views can be accessed and created via the left-side panel of the nav.
- The first line of every StickyNotebook is treated as the title and presented in a larger font size.
- Choose a color for each StickyNotebook from a range of tastefully-selected colors representing a pastel color palette.

## Technical Goals:

Make a React App that demonstrates common intermediate-level design patterns like higher-order components, custom hooks, component composition, recursive components, and layout components, building on and applying what I learned in the LinkedIn React Design Patterns course. Apply these patterns only after the core app structure is laid out to avoid premature optimizations. Demonstrate branded UI design.

## React Component Architecture

Nav Components viewable on all pages:

- File/Edit nav pane with import/export/download features
- Left Sidebar that contains "Views" or "StickyNotebook Workspaces"
- App logo
- Footer

Homepage Components:

- Searching Pane (full-text search, searches the contents of all notes reliably)
- Sorting Pane (sort by color, contents, creation date, or custom)
- StickyNotebooksListView (the wrapper holding StickyNotebook previews)
- StickyNotebookPreviewListItem

StickyNotebook View Components:

- TabPane
- TabSelector
- TextStylingPane
- StickyNotebookBody

## Data Model - Conceptual Design

### Common User Flow

A user creates a StickyNotebook. It is initialized with 1 tab. They enter text into that tab, style it and embed images or sounds. On a different page, they can collect StickyNotebooks they've created into StickyViews, where they can drag and resize their StickyNotebooks.

### Entities involved

User, StickyNotebook, UploadedImage, StickyView

### Relationships

User -> StickyNotebook (one-to-many)

StickyNotebook -> UploadedImage (one-to-many)

StickyView <-> StickyNotebook (many-to-many)

### Entity Attributes

#### User

- id
- username
- hashed password (with bcrypt via Auth0)

#### StickyNotebook

- id
- color
- tabs (a list of objects)
  - title (calculated and maintained using the text body)
  - formattedTextBody (markdown-style syntax. default font: Avenir)
  - currentTabVisibleInStickyNotebook
  - mediaToServe (a list of ObjectId references to UploadedImage. the formattedTextBody determines the placement and size)
- user (the ObjectId of the owner)

#### UploadedImage

- id
- filepath (temporary solution; in the future can use an s3 url instead)
- user (the ObjectId of the owner)

#### StickyView

- id
- name
- StickyNotebooks (a list with additional metadata)
  - width
  - height
  - (top, left) (position of top-left corner using CSS top/left properties)
  - currentTabVisibleInStickyView
  - minimized (true or false)
- user (the ObjectId of the owner)
