# Guide to Forking and Rebranding Firefox

This guide provides comprehensive information about forking Firefox, including licensing considerations, notable existing forks, and the technical process of rebranding.

## License Considerations

Firefox is released under the Mozilla Public License 2.0 (MPL-2.0). Key points:

- You can modify and redistribute Firefox code
- You must disclose source code for modified Mozilla files
- You must include original license and copyright notices
- You can combine MPL code with proprietary code
- Modifications must be available under the MPL
- Trademark restrictions apply to Firefox branding

## Notable Firefox Forks

Several successful Firefox forks demonstrate different approaches and focuses:

### LibreWolf
- Focus: Privacy and security
- Changes: Removes telemetry, hardens security settings
- Website: https://librewolf.net/

### Waterfox
- Focus: Performance and customization
- Changes: x64 optimization, classic add-on support
- Website: https://www.waterfox.net/

### Ghostery Browser
- Focus: Privacy and ad-blocking
- Changes: Built-in privacy features, custom search
- Website: https://www.ghostery.com/

### Floorp
- Focus: Feature-rich customization
- Changes: Additional UI features, performance tweaks
- Website: https://floorp.app/

## Rebranding Process

### 1. Build Environment Setup
```bash
# Clone Firefox source
git clone https://github.com/mozilla/gecko-dev.git
cd gecko-dev

# Install build dependencies
./mach bootstrap
```

### 2. Branding Directory Structure
Create your branding in `browser/branding/yourbrand/`:
```
yourbrand/
├── configure.sh          # Brand-specific variables
├── branding.nsi          # Windows installer branding
├── firefox.icns          # macOS icons
├── firefox.ico           # Windows icons
├── locales/             # Localized branding strings
└── content/             # Brand-specific content
```

### 3. Configure Build (mozconfig)
```bash
# Basic branding configuration
ac_add_options --with-app-name=YourBrand
ac_add_options --with-app-basename=YourBrand
ac_add_options --with-branding=browser/branding/yourbrand
ac_add_options --enable-official-branding

# Brand-specific variables
export MOZ_APP_DISPLAYNAME="Your Brand"
export MOZ_APP_VENDOR="Your Company"
```

### 4. Required Branding Assets
- Browser window icon (16x16, 32x32, 48x48, 64x64, 128x128)
- Application icon (macOS .icns, Windows .ico)
- About dialog logo
- Default browser icons
- Installer branding

## Patch Management System

Firefox forks often use patch management systems to maintain modifications across Firefox updates. Here's how it works:

### Why Use Patches?
1. **Maintainability**: Easy to update when new Firefox versions release
2. **Clarity**: Clear documentation of changes
3. **Modularity**: Organize changes by feature/component
4. **Reversibility**: Easy to remove specific changes

### Patch System Implementation (based on Ghostery's fern.js)

#### 1. Patch Organization
```
patches/
├── branding/           # Visual and name changes
├── privacy/            # Privacy-related modifications
├── features/           # Feature additions/changes
└── fixes/             # Bug fixes and tweaks
```

#### 2. Creating Patches
```bash
# Make changes in Firefox source
cd mozilla-release
# Make your changes
git add .
git commit -m "Description of changes"

# Create patch
git format-patch HEAD~1
mv *.patch ../patches/category/
```

#### 3. Applying Patches
```bash
# Example patch management script
./patch-manager.js import-patches  # Apply all patches
./patch-manager.js export-patches  # Export new changes as patches
```

#### 4. Patch Maintenance
- Keep patches small and focused
- Document each patch's purpose
- Test patches with each Firefox update
- Remove obsolete patches
- Track upstream changes that affect patches

## Automated Builds

Consider setting up CI/CD for:
1. Building on multiple platforms
2. Running tests
3. Creating installers
4. Updating patches
5. Release management

## Best Practices

1. **Stay Current**
   - Regularly merge Firefox security updates
   - Monitor Mozilla security advisories
   - Keep dependencies updated

2. **Documentation**
   - Document all modifications
   - Maintain changelog
   - Provide build instructions
   - Document patch purposes

3. **Testing**
   - Run Mozilla's test suite
   - Add tests for custom features
   - Test on all target platforms
   - Verify security features

4. **Community**
   - Provide clear contribution guidelines
   - Document development setup
   - Maintain issue tracker
   - Regular communication with users

## Resources

- [Firefox Source Documentation](https://firefox-source-docs.mozilla.org/)
- [Mozilla Build Instructions](https://firefox-source-docs.mozilla.org/setup/index.html)
- [Mozilla Public License 2.0](https://www.mozilla.org/en-US/MPL/2.0/)
- [Firefox Branding Guidelines](https://www.mozilla.org/en-US/styleguide/identity/firefox/branding/)
