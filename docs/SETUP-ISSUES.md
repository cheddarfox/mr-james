# Setup Issues Punch List

Issues discovered during initial project setup on 2026-01-12.

## Resolved

### 1. Script Execute Permission
- **File**: `scripts/init-project.sh`
- **Issue**: Script had Windows-style line endings (CRLF) causing "required file not found" error
- **Fix**: `sed -i 's/\r$//' ./scripts/init-project.sh`

### 2. Missing cspell Dictionary Words
- **Issue**: Several legitimate words missing from project dictionary
- **Words Added**: Speakability, Goodfellas, Unfilmable, throughlines, autofetch, Bren
- **Fix**: Added to `.cspell/project-words.txt`

## Pending / To Report Upstream

### 1. Line Ending Normalization
- **Recommendation**: Add `.gitattributes` to normalize line endings
- **Suggested Content**:
  ```
  * text=auto
  *.sh text eol=lf
  ```

### 2. Missing IMDb-Style README Template
- **Issue**: Project README should follow IMDb-style format, but template wasn't in `templates/`
- **Discovery**: Had to reference `seoul-identity` project to find the format
- **Recommendation**: Add `templates/readme-imdb-style.md` to upstream template
- **Status**: Template created for this project, needs upstream contribution

### 3. Pre-commit Hook npx Bug (BLOCKING)
- **Issue**: `npx lint-staged` in husky hook fails with "No matching version found for undefined@lint-staged"
- **Context**: Running `npx lint-staged` directly works fine (v15.5.2)
- **Environment**: WSL2 + Node v24.10.0 + npm 11.6.1
- **Workaround**: Use `git commit --no-verify` after manually running `npx lint-staged`
- **Attempted Fixes**:
  - Direct path `./node_modules/.bin/lint-staged` - Permission denied
  - `lint-staged` alone - Permission denied
  - `node node_modules/lint-staged/bin/lint-staged.js` - MODULE_NOT_FOUND (path resolution failure)
- **Root Cause**: WSL2/Husky environment path resolution differs from interactive shell
- **Recommendation**:
  - Investigate husky v9 + WSL2 compatibility
  - Consider downgrade to Node LTS (v22 or v20)
  - Or disable pre-commit hook and rely on CI validation

---

*Report issues to: https://github.com/cheddarfox/mr-james/issues*
