Bracket Battle - 2027 Update Guide
This guide will help you update Bracket Battle for the 2027 NCAA Men's Lacrosse Tournament.
Step 1: Get the 2027 Tournament Bracket
Visit https://www.ncaa.com/sports/lacrosse-men/d1 in early May 2027
Download or screenshot the official bracket
Note the 16 teams, their seeds, and first-round matchups
Step 2: Update Team Data in index.html
Open index.html and find the var T={} section (around line 633).
For Each Team, Update:
teamkey: {

  n: 'Team Name',        // Display name (keep short for mobile)

  r: '14-2',            // Season record

  s: '#1',              // Seed (or empty '' for unseeded)

  o: 26,                // Odds (1-100, higher = favored, get from ESPN)

  url: 'https://...',   // Team athletics site

  mascot: 'logos/teamkey.svg'  // Logo file (see Step 3)

}
Team Key Naming Convention:
Use lowercase, no spaces: notredame, pennstate, northcarolina
Keep consistent year-to-year when possible
If a team is new, create a logical key
Example Updates for 2027:
var T={

  // UPDATE SEED #1 (check 2027 bracket)

  maryland:     {n:'Maryland',   r:'15–1',s:'#1',o:28,url:'https://umterps.com/sports/mens-lacrosse',

    mascot:'logos/maryland.svg'},

  

  // UPDATE SEED #2

  princeton:    {n:'Princeton',  r:'13–2',s:'#2',o:26,url:'https://goprincetontigers.com/sports/mens-lacrosse',

    mascot:'logos/princeton.svg'},

  

  // ... continue for all 16 teams

};
Step 3: Update Team Logos
Download New Team Logos:
For each NEW team in the 2027 tournament:

Visit logos/README.md for Wikimedia Commons URLs
Search for: [Team Name] athletics logo svg
Download PNG or SVG (SVG preferred)
Save as logos/[teamkey].svg or .png

Update the file extension in index.html if using PNG:

mascot: 'logos/teamkey.png'  // if you use PNG instead of SVG
For Teams Already in 2026:
Keep the same logo files (princeton.svg, syracuse.svg, etc.)
No action needed unless logo changed
For Teams NOT in 2027:
Leave the logo files in the folder (doesn't hurt)
Or delete if you want to clean up
Step 4: Update First Round Matchups
Find the var FR=[] section (around line 669).

var FR=[

  {id:'fr1', a:'seed1team', b:'unseededteam', loc:'Location, ST', time:'Sat May 15 · 1:00pm', result:null, sortTime:TIMESTAMP},

  // ... 8 games total

];
Update for Each Game:
a: higher seed team key
b: lower seed team key
loc: host city from NCAA bracket
time: game date/time
sortTime: Unix timestamp (use https://www.epochconverter.com/)
Step 5: Update Quarterfinals, Semifinals, Finals
Update the QF, SF, and FIN objects to reference the correct first-round game IDs:

var QF=[

  {id:'qf1', frA:'fr1', frB:'fr2', loc:'Philadelphia, PA', time:'Sat May 22 · 12pm', result:null, sortTime:TIMESTAMP},

  // ... 4 games total

];

var SF=[

  {id:'sf1', qfA:'qf1', qfB:'qf2', loc:'Foxborough, MA', time:'Sat May 29 · 3pm', result:null, sortTime:TIMESTAMP},

  {id:'sf2', qfA:'qf3', qfB:'qf4', loc:'Foxborough, MA', time:'Sat May 29 · 6pm', result:null, sortTime:TIMESTAMP}

];

var FIN={id:'fin', sfA:'sf1', sfB:'sf2', loc:'Foxborough, MA', time:'Mon May 31 · 1pm', result:null, sortTime:TIMESTAMP};
Step 6: Update Deadline
Find var DEADLINE= (around line 611) and update to 2027 first-round start time:

var DEADLINE=new Date('2027-05-15T13:00:00-04:00'); // First game kickoff
Step 7: Update Tournament End Date
Find var TOURNAMENT_END= and update to 2027 championship game end time:

var TOURNAMENT_END=new Date('2027-05-31T18:00:00-04:00'); // After finals
Step 8: Update Open Graph Image
If you want to create a new 2027 social sharing image:

Design new og-image.png with "2027 NCAA Men's Lacrosse"
Replace the existing og-image.png file
Update meta tag if filename changes
Step 9: Test Locally
Open index.html in a browser
Check that all 16 team names and logos display correctly
Verify matchups are correct
Test bracket selection flow
Test leaderboard displays properly
Step 10: Clear Database (Important!)
Before going live with 2027:

Log into Supabase dashboard
Clear all data from:
brackets table
scores table
comments table
This gives everyone a fresh start
Step 11: Deploy
Commit all changes to Git:

git add index.html logos/

git commit -m "Update for 2027 NCAA Men's Lacrosse Tournament"

git push origin main

GitHub Pages will auto-deploy to: https://ehall4444.github.io/bracket-battle/
Quick Checklist
16 teams updated in var T={}
All team logos downloaded to /logos/
First round (FR) matchups updated
Quarterfinals (QF) references correct
Semifinals (SF) references correct
Finals (FIN) references correct
DEADLINE date updated
TOURNAMENT_END date updated
Tested locally
Database cleared
Deployed to GitHub Pages
Tips for Future Years
Save the 2026 version: Copy index.html to index-2026-backup.html before editing
Team records: ESPN has final records usually by tournament time
Odds: Check ESPN's "Tournament Challenge" or Vegas odds
Timestamps: Use Eastern Time (tournament is usually East Coast)
Need Help?
NCAA bracket: https://www.ncaa.com/sports/lacrosse-men/d1
Team logos: See logos/README.md
Epoch converter: https://www.epochconverter.com/
Test bracket locally before deploying

Good luck with the 2027 tournament! 🏆

