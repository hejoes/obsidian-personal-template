Obsidian is my favourite note-taking app. I'm grateful to have had the
opportunity to use it freely over the last years. But the out-of-the-box version
felt too basic and bland for my personal use. Although it has great core
features, it felt like a 6/10 experience in general since I didn't like the
default look and I didn't have a managed way of accessing the notes and
important information.

Overall personal disorganization led to situations where it was less convenient
for me to plan ahead since I didn't have good visibility of tasks scheduled for
the future. Writing down quick-facts and new information felt tedious as I
always had to navigate to the correct files going through the file explorer and
creating the file taking multiple steps.

The following Obsidian starting-setup addresses these concerns by implementing
some of the best-practices when it comes to personal note-taking. Keep in mind
that the following setup works well for me personally but is not universal
across all use-cases.

I also recommend Obsidian Sync to keep your mobile and computer always up to
date. Google Drive/OneDrive sync also works well for syncing regular notes, but
in my experience I faced major challenges keeping the plugins and personal
configuration in sync. Synchronization failures happened more often between
frequent switches from mobile to computer and Obsidian mobile couldn't fetch
latest computer plugins/configuration without crashing. Based on these factors,
Obsidian Sync for €10/month was worth it for me as the changes are reflected
live (over interval/manual sync) and the device compatibility works (almost)
flawlessly.

**Overview**: ![](https://i.imgur.com/m3SmHvD.png)

## Getting started

- Copy/Replace all files from `.obsidian/` in this repository to your
  `vault-name/.obsidian` to get the Theme & Plugin configurations. Before
  proceeding, make sure you are not replacing your important custom setup that
  you want to keep.
- Copy `Home.md` to the root of your vault. This is the main dashboard note.
- Copy the `Notes/Templates/` folder to your vault. This contains the daily note
  template used by Periodic Notes and Templater.

## Plugins

Following plugins are necessary for all features to work:

1. [**Calendar**](https://obsidian.md/plugins?id=calendar) - allows displaying
   calendar widget. Useful for multi-day navigation. Used in `Home.md`
2. [**Dataview**](https://obsidian.md/plugins?id=dataview) - powers all dynamic
   content across the vault (todo lists, file listings, navigation). DataviewJS
   is enabled for advanced JavaScript queries used in `Home.md` and daily notes
3. [**Periodic Notes**](https://obsidian.md/plugins?id=periodic-notes) - handles
   daily note creation with the correct folder structure
   (`Notes/YYYY/MMMM/DD ddd`) and applies the template from
   `Notes/Templates/daily.md`
4. [**Templater**](https://obsidian.md/plugins?id=templater-obsidian) - runs
   dynamic logic at note creation time such as rollover of incomplete todos and
   file creation triggers.
5. [**ICS**](https://obsidian.md/plugins?id=ics) - fetches upcoming events from
   Google Calendar. Used in `Home.md` to display the next 7 days of events.
   included under icons/devicons. Have to be added via settings.

Good to have:

1. [**Obsidian Imgur Plugin**](https://obsidian.md/plugins?id=obsidian-imgur-plugin) -
   enables pasting images directly into notes and auto-uploading them to Imgur
2. [**Iconize**](https://obsidian.md/plugins?id=obsidian-icon-folder) - allows
   adding custom icons to folders and files in the file explorer to make your
   Vault more personalized. This repository includes Devicons, Font Awesome
   Brands, and Simple Icons packs in `.obsidian/icons/`.
3. [**Recent Files**](https://obsidian.md/plugins?id=recent-files-obsidian) -
   adds a sidebar panel showing recently opened files for quick access
4. [**Style Settings**](https://obsidian.md/plugins?id=obsidian-style-settings) -
   allows overriding the Border theme's styling (colors, dark mode). See Theme
   section below
5. [**Omnisearch**](https://obsidian.md/plugins?id=omnisearch) - when mapped to
   `cmd + shift + f`, adds more comprehensive vault search across all files.
6. [**Iconize**]() - custom icons for folders and files. Custom Devicons are
   available under `.obsidian/icons/devicons` Have to be enabled in settings.

## Features

### Theme

- The Theme used is `Border`
- `Style Settings` community plugin is used for overrides:
  - Editor->Headings main color: `#A5CDF9`
  - Appearance (dark mode)->Enable
  - Appearance (dark mode)->Color->Accent color: `#C04242`

### Home note

`Home.md`

- Home note acts as a central navigation system between all the important notes
  for the user. It allows quick & convenient overview across upcoming events
  (fetched from Google Calendar), unfinished todos, quick-link to daily note and
  keeps track of previously edited/watched files from important links/folders

Home file consists of the following sections:

#### Quick-link to daily note

- On the top of the home page is link/creation to Today's note:
  ![](https://i.imgur.com/a1rdHR3.png)

#### Todos

- Unfinished Todos are automatically fetched from the previous daily note:
  ![](https://i.imgur.com/W5Ct4xK.png)

#### Upcoming events

- Last 7-day events from Google Calendar are always automatically fetched and
  displayed: ![](https://i.imgur.com/2ydOprC.png)

#### Quick-link widgets

- Important files are quickly accessible with possibility to create new note in
  existing folders on the go: ![](https://i.imgur.com/sFxZCHC.png)

### Daily note system

#### Intelligent Today/Tomorrow navigation

- Even when multiple days have passed after last daily note creation, the logic
  automatically finds the previous/future daily note and allows navigating to
  it. In the current example Tomorrow's note does not exist but navigation
  allows jumping directly to the latest daily note:
  ![](https://i.imgur.com/5mTaXLx.png)

#### Intelligent Todos

- All unfinished Todos will get transferred over to the next daily note. Todos
  will also be displayed live in the Home file.

Todos from previous note: ![](https://i.imgur.com/m42OkSt.png)

And the second todo which was previously completed will not be included in the
next daily note: ![](https://i.imgur.com/NUOwADg.png)

## More tips

1. You can add Obsidian widget pointing to Home file on your phone's main
   screen.
2. Since the mobile Obsidian version does not work well with Imgur, pasted
   images default to being stored in the vault root which gets cluttered
   quickly. To keep things organized, create an new `assets` folder in your
   vault and change the storage location in Settings -> Files & Links -> Default
   location for new attachments -> `assets`.
3. You can enable Vim mode under Editor->advanced. However be prepared since
   Obsidian ask's you how to quit Vim before allowing it.
4. Use Quick-Note folder over root as default file creation location.
