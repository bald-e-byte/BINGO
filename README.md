Bingo: Multiplayer Edition

A two-player, real-time Bingo game for your phone. Build your own board, take turns calling numbers, and be the first to complete 5 lines.

Built with plain HTML, CSS and JavaScript, plus Firebase Realtime Database for live sync. There is no build step and no backend server.

Play it: https://bald-e-byte.github.io/BINGO/

How to play

1. Enter your name. One player taps Create a room and shares the 4-letter code. The other player enters it and taps Join room.
2. Build your board. Tap a square, then tap a number. Use every number from 1 to 25 exactly once. Randomize fills the board for you. Tap Lock board when you're happy.
3. When both boards are locked, the game starts. The room creator calls first, then you alternate.
4. On your turn, tap a number in the Caller grid. It gets crossed out for both players and the turn passes.
5. Mark your board. When a number is called, it turns yellow on your board if you have it. Tap it to mark it green.
6. Each completed line (row, column or diagonal) lights up a letter of B-I-N-G-O.
7. At 5 lines, the red SHOUT BINGO! button appears. Tap it to claim the win.

Rules

• Exactly 2 players per room.
• The board is a 5x5 grid with the numbers 1 to 25, each used once.
• There are 12 possible lines: 5 rows, 5 columns and 2 diagonals.
• A number can only be called once, and only by the player whose turn it is.
• The first player to claim BINGO with 5 real lines wins. A false claim loses the game.

Project structure

index.html            the whole game: UI, styles and game logic
database.rules.json   Firebase security rules (publish these in the console)
README.md             this file

Setup

1. Create a Firebase project at https://console.firebase.google.com.
2. Add a Web app (</>) and copy its firebaseConfig into the top of the script in index.html.
3. Enable Anonymous sign-in under Build, Authentication, Sign-in method.
4. Create a Realtime Database and start in locked mode.
5. Publish the rules: open the database's Rules tab, paste in all of database.rules.json, and press Publish.
6. Host it. Either use GitHub Pages (Settings, Pages, deploy from main and / (root)) or Firebase Hosting.
7. Authorize your domain under Authentication, Settings, Authorized domains. Add the bare domain only, for example bald-e-byte.github.io, with no https:// and no path. localhost is already allowed for local testing.

To test locally, run npx serve in the project folder and open the localhost address on two devices or browser tabs. Opening index.html straight from the file system won't work.

How it works

Each game lives at rooms/{code} in the database:

Path
Holds
host, guest
the two players' anonymous IDs
players/{uid}
display names
boards/{uid}
each player's 25-number board
ready/{uid}
set once a player locks their board
status, turn
whether the game has started, and whose turn it is
called/{n}
numbers called so far, and who called them
claim
who shouted BINGO

Calling a number writes called/{n} and flips turn in a single atomic update, so the two can never get out of sync.

Anti-cheat

Realtime Database has no server code, so the security rules do the enforcing:

• No duplicates: a board needs exactly the keys 0 to 24, each a number from 1 to 25. The app prevents repeated numbers while you build.
• No skipping turns: only the player whose turn it is can call a number.
• No double calling: a number can only be written once.
• Hidden boards: you can't read your opponent's board until someone claims BINGO.
• Verified claims: after a claim, both boards unlock and both phones check that the claimer's board is valid and has 5 lines among the called numbers.

Limitation: because there's no server referee, a determined cheater could tamper with their own client. A fully trusted referee would need a Firebase Cloud Function, which requires the paid Blaze plan.

Known limitations

• Refreshing the page leaves the room. Rejoining a game in progress isn't supported yet.
• Marked squares are stored on your device only.
• Exactly 2 players per room.

Ideas for later

• Rejoin a game after a refresh
• Rematch button that keeps the same room
• Sound and vibration when your letters light up
• A Cloud Function referee for fully server-verified claims

Tech

HTML, CSS, JavaScript (ES modules), Firebase Authentication (anonymous) and Firebase Realtime Database. The font is Patrick Hand.

Credits

Made by Bald-e.
