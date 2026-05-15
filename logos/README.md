# Team Logos for Bracket Battle

This directory contains team mascot logos for all tournament teams.

## Quick Setup for 2026 Tournament

### Option 1: Download Logos Manually (Recommended)
Visit each URL below and save the image to this `logos/` folder with the corresponding filename:

#### Seeded Teams (Top 8)
- **princeton.png** - https://commons.wikimedia.org/wiki/File:Princeton_Tigers_logo.svg (download PNG)
- **notredame.png** - https://commons.wikimedia.org/wiki/File:Notre_Dame_Fighting_Irish_logo.svg
- **northcarolina.png** - https://commons.wikimedia.org/wiki/File:North_Carolina_Tar_Heels_logo.svg
- **richmond.png** - https://commons.wikimedia.org/wiki/File:Richmond_Spiders_logo.svg
- **virginia.png** - https://commons.wikimedia.org/wiki/File:Virginia_Cavaliers_sabre_logo.svg
- **syracuse.png** - https://commons.wikimedia.org/wiki/File:Syracuse_Orange_logo.svg
- **cornell.png** - https://commons.wikimedia.org/wiki/File:Cornell_Big_Red_Athletics_logo.svg
- **pennstate.png** - https://commons.wikimedia.org/wiki/File:Penn_State_Nittany_Lions_logo.svg

#### Unseeded Teams
- **marist.png** - https://commons.wikimedia.org/wiki/File:Marist_Red_Foxes_logo.svg
- **jacksonville.png** - https://commons.wikimedia.org/wiki/File:Jacksonville_Dolphins_logo_2018.svg
- **albany.png** - https://commons.wikimedia.org/wiki/File:Albany_Great_Danes_logo.svg
- **army.png** - https://commons.wikimedia.org/wiki/File:Army_Black_Knights_logo.svg
- **johnshopkins.png** - https://commons.wikimedia.org/wiki/File:Johns_Hopkins_Blue_Jays_logo.svg
- **duke.png** - https://commons.wikimedia.org/wiki/File:Duke_Blue_Devils_logo.svg
- **georgetown.png** - https://commons.wikimedia.org/wiki/File:Georgetown_Hoyas_logo.svg
- **yale.png** - https://commons.wikimedia.org/wiki/File:Yale_Bulldogs_logo.svg

### Option 2: Browser Extension Method
1. Install a browser extension like "Download All Images" or "Image Downloader"
2. Visit https://commons.wikimedia.org/wiki/Category:NCAA_Division_I_athletics_logos
3. Search for each team name
4. Batch download all logos

### Option 3: Command Line (if you have wget or curl with proper headers)
```bash
# Example - you may need to adjust headers/cookies
wget -O princeton.png "https://upload.wikimedia.org/wikipedia/commons/6/60/Princeton_Tigers_logo.svg"
```

## Sizing Guidelines
- Recommended size: 40-60px height for in-bracket display
- Format: PNG or SVG (PNG is easier for browser compatibility)
- Transparent background preferred

## For Future Years (2027+)
1. Check NCAA.com for the tournament bracket
2. Update the team list in `index.html` (lines ~613-650)
3. Download new team logos as needed
4. Keep the same filename pattern: `{teamkey}.png`

## Color Codes (for reference)
Each team's primary colors are documented in index.html for potential future use:
- Princeton: Orange/Black (#FF8F00, #000000)
- Notre Dame: Gold/Blue (#C99700, #0C2340)
- North Carolina: Carolina Blue (#7BAFD4)
- Syracuse: Orange (#F76900)
... etc.

## License Notes
All logos from Wikimedia Commons are either:
- Public domain (PD-textlogo)
- CC-BY-SA licensed
- Educational/fair use

Always attribute to the respective universities when required.
