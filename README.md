# StickyNotebooks

StickyNotebooks are a new way to organize your thoughts to help you collect text, gather your research, write brainstorms, and be prepared with the subset of information you need. Picture Evernote, but with additional drag-and-drop views that help you pull together subsets of your notes for simultaneous viewing. Picture Mac Stickies, but with tabbable views that help you deep-dive on topics of your interest.

Project started 8/24/22.

StickyNotebooks will be a React / Express.js project.

## App description:

- On the homepage you can see a searchable, sortable list of your StickyNotebook previews. You click on one and it takes you to that StickyNotebook page.
- The StickyNotebook page is a tabbed interface of text blocks. It's easy to create new tabs inside of a single StickyNotebook that collect related information (the "Notebook" view of a StickyNotebook). Default font: Avenir.
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

- Searching Pane (full-text search, searches the contents of ALL notes reliably, unlike Mac Stickies)
- Sorting Pane (sort by color, contents, creation date, or custom)
- StickyNotebooksListView (the wrapper holding StickyNotebook previews)
- StickyNotebookPreviewListItem

StickyNotebook View Components:

- TabPane
- TabSelector
- TextStylingPane
- StickyNotebookBody

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.
