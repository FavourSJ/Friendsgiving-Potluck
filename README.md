# Friendsgiving Potluck 2026

Static site (GitHub Pages). Edit the `CONFIG` and `GROUPS` blocks at the top of the `<script>` in `index.html`.

## Going live (so everyone sees the same country picks)
1. console.firebase.google.com -> Create project -> Build -> Realtime Database -> Create (pick Europe).
2. Rules tab, replace everything with this and Publish:
   ```json
   {
     "rules": {
       "claims": {
         ".read": true,
         "$gid": {
           ".write": "newData.exists() && now < 1795737600000",
           ".validate": "$gid.matches(/^g[1-7]$/) && newData.hasChildren(['country','ts']) && newData.child('country').isString() && newData.child('country').val().length < 60"
         }
       }
     }
   }
   ```
   `1795737600000` is midnight at the start of Friday 27th November 2026 (UTC, same as London in winter).
   After that moment nobody can add or change a pick, in the page or outside it.
   To move the date, change `lockISO` and `lockText` in `index.html` and this number
   (milliseconds since 1970, e.g. from https://www.epochconverter.com).
3. Copy the database URL (https://...firebasedatabase.app) into `CONFIG.firebaseUrl` in `index.html`.

That's it. The courses are in `GROUPS` in `index.html`, so nothing else needs setting up.

## Groups and courses
Set in `GROUPS`. `team: "pud"` marks the groups that are allowed to share one country
(Kieran, Monty and Anvay), so they can cook together or separately. Remove the `team` key
from a group to make its country exclusive like everyone else's.

## Photos
Put `photos/<firstname>.jpg` (lowercase, e.g. `photos/kieran.jpg`) for the crew cards.
Add entries to `GALLERY` for the "Previous trips" strip.

## Local testing
Without a `firebaseUrl` the page runs in demo mode: picks save in your own browser only.
Clear the site data in your browser to reset, including the saved "who am I" name.
