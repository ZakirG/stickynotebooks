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

## Data Model

### Common User Flow

A user creates a StickyNotebook. It is initialized with 1 tab. They enter text into that tab, style it and embed images (or sounds in the future). On a different page, they can collect StickyNotebooks they've created into StickyViews, where they can drag and resize their StickyNotebooks.

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
- tabs (a list of embedded documents)
  - title (calculated and maintained using the text body)
  - formattedTextBody (markdown-style syntax. default font: Avenir)
  - plainTextBody (duplicating some data in order to support full-text search. needs a text index)
  - currentTabVisibleInNotebookView (index of the tab to show in notebook view)
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
  - StickyNotebook (ObjectId of the StickyNotebook)
  - width
  - height
  - (top, left) (position of top-left corner using CSS top/left properties)
  - currentTabVisibleInStickyView (index of the tab to show in sticky view)
  - minimized (true or false)
- user (the ObjectId of the owner)

### MongoDB Implementation Discussion

We'll need to add a "text index" on the plainTextBody to support full text search. Unfortunately, this only supports keyword searching -- partial word searches won't work.
`{ $text : {$search : "phrase to locate"}}`

We choose to embed the individual tabs of a StickyNotebook inside of the StickyNotebook document, rather than storing them in separate documents. When a user opens a notebook, they'll switch tabs freely, so we'll want all that data accessed together. We can ensure quick read-times with an embed.

In the proposed data structure, opening a StickyView will consistently result in $lookups to obtain the text body of the member StickyNotebooks, which is a relatively expensive operation, although one StickyView is likely to have less than 10 members. We could avoid lookups by duplicating the text body of member StickyNotebooks inside of all StickyView parents, but when a user edits a StickyNotebook from the StickyView, we'd then need to perform a lookup to edit the text body in the StickyNotebook document anyway, and maintain that data in other StickyViews where the text body is present. The [MongoDB docs](https://www.mongodb.com/developer/products/mongodb/schema-design-anti-pattern-massive-arrays/) recommend to avoid duplicating data that will be frequently updated. So we choose to keep the StickyNotebook text body only in the StickyNotebook document.
