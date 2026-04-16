# AI Agent Guidance for Glasgow Caledonian Philatelic Society Website

## Project Overview

Static Jekyll site for the Glasgow Caledonian Philatelic Society, a philatelic organization in Glasgow. The site contains historical information, meeting schedules, competition records, and educational content about stamp collecting and philately.

**Tech Stack**: Jekyll (jekyll-theme-minimal), Markdown, HTML/CSS

**Repository Structure**:
- Root: Content markdown files (`.md`)
- `_layouts/`: HTML templates (`default.html`)
- `_includes/`: Reusable components (`menu.md`)
- `stylesheets/`: CSS styling (`styles.css`)
- `images/`: Image assets (PNG, JPG, etc.)
- `pdfs/`: PDF documents
- `.github/skills/`: Custom skills directory
- `_config.yml`: Jekyll configuration
- `.cspell.json`: Spelling checker configuration

## Key Conventions

### Content Organization
- **Landing**: `index.md` (homepage with subscription info, upcoming events)
- **History**: `history.md` (index) → `history-caledonian.md`, `history-glasgow.md` (detailed)
- **Meetings**: `meetings.md` (current syllabus) + `gc-previous-meetings.md`, `previous-meetings.md` (archives)
- **Competitions**: `competitions-gc.md` (current) + `previous-competitions.md` (archives)
- **Organization**: `committee.md`, `past-presidents.md`, `about.md`
- **Collections**: `forgery-collection.md`, `postcard-collecting.md`, `uddingston-stamps.md`
- **Support**: `links.md`, `location.md`, `contactus.md`, `advice-for-visiting-speakers.md`

### Link Conventions
- Use relative paths with `./` prefix: `[Link Text](./target-file)`
- Link to markdown files without `.md` extension in references
- Internal links should reference the filename stem (e.g., `./history` not `./history.md`)

### Image References
- Store images in `images/` directory
- Reference as: `![Alt Text](./images/filename.jpeg)` or `![Alt Text](images/filename.jpeg)`
- Use relative paths consistent with Jekyll rendering

### Table Formatting
- Use standard markdown tables (with `|` and `-`)
- Tables are heavily used for:
  - Meeting schedules (Date | Subject | Displayer | Location | Timing)
  - Competition results (Year | Winner | Entry)
  - Past presidents (Year | Name)
  - Trophy records (tables with competition details)
- Ensure consistent column alignment and spacing

### Jekyll Front Matter
- Not explicitly required for content files
- Default layout applied to all files via `_config.yml`
- Override layout per file if needed: `---\nlayout: custom\n---`

## Common Editing Patterns

### Adding/Updating Meeting Records
- File: `meetings.md` or `gc-previous-meetings.md`
- Format: Markdown table with columns for Date, Subject, Displayer, Location, Timing
- Dates: Use format like "2 October", "23 October"
- Location codes: **P** (afternoon/Partick), **U** (evening/University), etc.

### Adding/Updating Competition Results
- File: `competitions-gc.md` or `previous-competitions.md`
- Format: Markdown table with Year | Winner | Entry
- Maintain chronological order (most recent first for current, oldest first for archives)

### Updating Committee Information
- File: `committee.md`
- Format: Key-value pairs with bold position names and member names
- Note: Members in bold are automatically on committee per the instructions

### Spelling and Content Quality
- Use skill: `.github/skills/spell-check/` 
- Command: `npx cspell "*.md"` to check all top-level files
- Dictionary: `.cspell.json` contains project-specific terms (names, philatelic terms, place names)
- Note: CSpell configuration already includes common members' names and philatelic terminology

## Project-Specific Terminology

**Key Organizations**:
- GCPS: Glasgow Caledonian Philatelic Society (merged entity, formed 2025)
- CPS/Caledonian: Caledonian Philatelic Society (now merged)
- GPS/Glasgow: Glasgow Philatelic Society (now merged)
- FRPSL: Fellow of the Royal Philatelic Society of London
- ASPS: Association of Scottish Philatelic Societies
- ABPS: Association of British Philatelic Societies

**Key Locations**:
- Vine Conference Centre, Dunfermline (ASPS Congress venue)
- Dewar's Centre, Perth (past Congress venue)
- Glasgow Stamp Shop (current venue, West Nile Street)
- Partick Burgh Halls (afternoon meeting venue)
- Graham Hills Building, Strathclyde University (legacy venue - outdated)

**Key Events**:
- Annual Congress of ASPS (April)
- Competition Night
- Bourse (buying/selling event)
- Alphabet Lottery
- President's Night
- Auction Night
- Family Day at Kelvin Hall

## Potential Issues & Solutions

### Common Pitfalls

1. **Broken Links**
   - Problem: Renaming files without updating cross-references
   - Solution: Search for all `.md` files referencing the old filename
   - Pattern: Look for `](./old-name)` or `[text](old-name)`

2. **Image Path Issues**
   - Problem: Images not rendering due to incorrect paths
   - Solution: Use `./images/filename` format consistently
   - Verify image files exist in `images/` directory

3. **Table Formatting Breaks**
   - Problem: Markdown tables break due to misaligned pipes or spaces
   - Solution: Ensure consistent column count and pipe placement
   - Test: Render locally with Jekyll to verify

4. **Spelling Errors in Names**
   - Problem: Member names and place names spelled inconsistently
   - Solution: Check `.cspell.json` for approved terms
   - Add new proper nouns to `.cspell.json` if frequently misspelled

5. **Outdated Venue Information**
   - Problem: References to Graham Hills Building (no longer used)
   - Solution: Update venue references to Glasgow Stamp Shop or Partick Burgh Halls
   - Check: `advice-for-visiting-speakers.md` has location-specific guidance

6. **Front Matter Issues**
   - Problem: Pages not rendering with correct layout
   - Solution: Verify front matter syntax (`---` delimiters) or rely on `_config.yml` defaults
   - Usually not needed—Jekyll applies default layout automatically

### Quick Checks Before Completing Content Updates

- [ ] All links use relative paths with `./` prefix
- [ ] Image paths are correct (prefix with `./images/`)
- [ ] Markdown tables have consistent column counts
- [ ] Proper nouns and member names are spelled correctly (verify against `.cspell.json`)
- [ ] File names are lowercase with hyphens (kebab-case)
- [ ] No broken cross-references after file renames

## Available Skills

- **spell-check**: Check spelling in markdown files
  - Location: `.github/skills/spell-check/SKILL.md`
  - Configuration: `.cspell.json`
  - Usage: Follow the spell-check skill documentation

- **link-validator**: Validate internal links and image references
  - Location: `.github/skills/link-validator/SKILL.md`
  - Validates markdown file cross-references and image paths
  - Prevents broken links in navigation and content
  - Usage: Follow the link-validator skill documentation

## Development Workflow

1. **Local Testing**: Build site locally with Jekyll to preview changes
   - Command: `bundle exec jekyll serve` (if bundled)
   - Or: `jekyll serve` (if Jekyll installed globally)
   - Access: `http://localhost:4000`

2. **Content Validation**: Use spell-check skill for content quality
   - Run before committing: `npx cspell "*.md"`

3. **Link Validation**: Scan files for broken references after edits
   - Check for typos in relative link paths
   - Verify renamed files are updated everywhere they're referenced

## Merge Notes

The repository documents the merger of Caledonian Philatelic Society and Glasgow Philatelic Society into Glasgow Caledonian Philatelic Society (GCPS) as of 2025. Some historical content refers to the separate societies. When updating:
- Current information should reference GCPS
- Historical records preserve the original society names
- Venue change occurred in 2026 (moved to Glasgow Stamp Shop)
