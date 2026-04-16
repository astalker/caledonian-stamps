# Link Validator Skill

Validate internal links, image paths, and cross-references in markdown files to ensure content integrity.

## Purpose

This skill helps identify broken links, missing image files, and incorrect cross-references that could break the site navigation or user experience.

## Scope

This skill applies to:
- Internal links between markdown files: `[text](./filename)` patterns
- Image references: `![alt text](./images/filename.ext)` patterns
- Cross-references between pages
- All `.md` files in the repository

## How to Use

### Using VS Code Find & Replace

1. **Find Broken Link Patterns**
   - Open Find & Replace (Ctrl+H)
   - Search for pattern: `\]\(\.\/([a-z\-]+)\)`
   - This finds all internal links with format `[text](./filename)`
   - Manually verify each reference points to an existing file

2. **Find All Image References**
   - Search for pattern: `!\[.*?\]\(\.\/images\/`
   - Verify each image file exists in the `images/` directory
   - Check file extensions match (case-sensitive on some systems)

### Using Command Line

**Method 1: Find broken markdown references**
```bash
# Find all .md file references and check if they exist
# Search for patterns like ](./filename)
grep -r "\]\(\.\/[a-z\-]*\)" *.md

# Then verify each file exists:
ls history-caledonian.md  # should exist
ls invalid-file.md        # will show error if missing
```

**Method 2: Find all image references**
```bash
# List all image references in markdown files
grep -r "!\[" *.md | grep images

# List actual images in directory
ls -la images/
```

**Method 3: PowerShell script to validate links**
```powershell
# Validate that referenced files exist
$mdFiles = Get-ChildItem -Filter "*.md" -Recurse

foreach ($file in $mdFiles) {
    $content = Get-Content $file.FullName
    
    # Find all internal links [text](./filename)
    $links = [regex]::Matches($content, '\]\(\.\/([a-z\-]+)\)')
    
    foreach ($link in $links) {
        $target = $link.Groups[1].Value
        $targetFile = "$($file.Directory)\$target.md"
        
        if (-not (Test-Path $targetFile)) {
            Write-Host "BROKEN LINK: $($file.Name) -> $target (file not found)"
        }
    }
}
```

## Common Link Patterns in This Project

### Valid Examples
```markdown
# Correct internal link
[Back to History](./history)
[View Competitions](./competitions-gc)

# Correct image reference
![Logo](./images/cpslogo.jpg)
![Room Layout](images/room-layout.png)
```

### Invalid Examples
```markdown
# Incorrect - missing ./
[Back to History](history)

# Incorrect - includes .md extension
[View Competitions](./competitions-gc.md)

# Incorrect - wrong path prefix
[Back](../history)

# Incorrect - image not in ./images/
![Logo](cpslogo.jpg)
```

## Files Frequently Referenced

These files are linked from multiple places and must exist:
- `history.md` - Linked from history pages
- `meetings.md` - Linked from navigation and other pages
- `competitions-gc.md` - Current competitions page
- `committee.md` - Committee information
- `advice-for-visiting-speakers.md` - Speaker guidance
- `gc-previous-meetings.md` - Meeting archives

## Image Files to Verify

Common images referenced in content:
- `images/cpslogo.jpg` - Site logo
- `images/room-layout.png` - Meeting room diagram
- `images/forgery-*.jpeg` - Forgery collection images
- `images/Clutha-Bar.jpg`, `Old-Fish-Market.jpg`, etc. - Historical photos

## Validation Checklist

Before committing content updates:

- [ ] All `[text](./filename)` links point to existing `.md` files in root directory
- [ ] No links include `.md` extension: use `./history` not `./history.md`
- [ ] All `![alt](./images/filename)` references have files in `images/` directory
- [ ] Image filenames match exactly (case-sensitive on Unix-like systems)
- [ ] No broken cross-references after file renames
- [ ] Internal links use `./` prefix for relative paths
- [ ] Navigation links in `_layouts/default.html` match actual file names

## Common Issues & Solutions

### Issue: Link to file that doesn't exist
**Symptom**: `[text](./nonexistent-page)` but no `nonexistent-page.md` file
**Solution**: 
- Verify the target file name is spelled correctly
- Check the actual file name in the repository
- Ensure the file is in the root directory (not in a subdirectory)

### Issue: Image file not found
**Symptom**: `![alt](./images/missing.jpg)` but file doesn't exist
**Solution**:
- Verify image file exists in `images/` directory
- Check file extension matches exactly (case-sensitive)
- Confirm image path doesn't include extra `images/images/`

### Issue: Link includes .md extension
**Symptom**: `[text](./history.md)` 
**Solution**: Remove `.md` extension → `[text](./history)`

### Issue: Link missing ./ prefix
**Symptom**: `[text](history)` instead of `[text](./history)`
**Solution**: Add `./` prefix for proper relative path resolution

### Issue: Broken link after file rename
**Symptom**: File renamed from `gc-meetings.md` to `meetings.md`, but old link still used
**Solution**: 
- Use Find & Replace to update all occurrences
- Search: `](./gc-meetings)`
- Replace: `](./meetings)`
- Verify all references updated

## Testing Links Locally

1. **Build site locally with Jekyll**:
   ```bash
   jekyll serve
   ```
   - Access at `http://localhost:4000`
   - Click through all navigation links
   - Verify images render correctly

2. **Use browser developer tools**:
   - Press F12 in browser
   - Check Console for 404 errors
   - Check Network tab for broken image requests

3. **Automated link checking** (external tools):
   - Run `html-proofer` if added to project
   - Command: `htmlproofer ./_site`

## Integration with Other Skills

- **Spell Check**: After validating links, run spell-check to catch content errors
- **Content Updates**: When adding new meeting records or competitions, validate new cross-links

## Tools & Resources

- [VS Code Find & Replace](https://code.visualstudio.com/docs/editor/codebasics#_find-and-replace) - Built-in regex support
- [jekyll-link-checker](https://github.com/wjdp/htmlproofer) - Automated link validation for Jekyll sites
- [Regular Expressions](https://regex101.com/) - Test regex patterns for link extraction
