# iOS IPA Build Actions

This repository includes two GitHub Actions for building iOS IPA files:

## 1. Auto Build & Release iOS IPA (`Auto Build & Release iOS IPA.yml`)

### Triggers:
- **Automatic**: When a tag starting with 'v' is pushed (e.g., `v1.9.4`)
- **Manual**: Can be triggered manually via workflow_dispatch with optional version tag input

### Features:
- Builds unsigned iOS IPA using Flutter 3.22.x
- Creates GitHub release with changelog
- Uploads IPA as artifact and to release
- Uses version information from `assets/app_version.json`

### Usage:
```bash
# Create and push a tag to trigger automatic release
git tag v1.9.4
git push origin v1.9.4
```

Or trigger manually from GitHub Actions tab.

## 2. Manual iOS IPA Build (`ios-manual-build.yml`)

### Triggers:
- **Manual only**: Via GitHub Actions workflow_dispatch

### Features:
- Choose build type (release/debug)
- Option to create GitHub release or just build artifacts
- Version override capability
- Detailed build information in release notes

### Input Parameters:
- `build_type`: Choose between 'release' or 'debug' (default: release)
- `create_release`: Whether to create a GitHub release (default: true)
- `version_override`: Optional version override (uses app version if empty)

### Usage:
1. Go to the "Actions" tab in your GitHub repository
2. Select "Manual iOS IPA Build"
3. Click "Run workflow"
4. Configure the input parameters as needed

## Code Signing Setup (Optional)

Both workflows create unsigned IPA files by default. To enable code signing:

1. Add the following secrets to your repository:
   - `APPLE_CERT_BASE64`: Base64 encoded iOS distribution certificate (.p12)
   - `APPLE_CERT_PASSWORD`: Password for the certificate
   - `PROVISIONING_PROFILE_BASE64`: Base64 encoded provisioning profile
   - `KEYCHAIN_PASSWORD`: Password for the build keychain

2. Uncomment the code signing steps in the workflows

3. Replace the unsigned build steps with signed build steps

## File Structure

```
.github/workflows/
├── Auto Build & Release iOS IPA.yml    # Automatic tag-based releases
├── ios-manual-build.yml                 # Manual build with options
└── ...

assets/
├── app_version.json                     # Main app version info
└── ...

simple_live_app/
├── ios/                                 # iOS-specific files
├── pubspec.yaml                        # Flutter dependencies
└── ...
```

## Version Management

The workflows use `assets/app_version.json` for version information:

```json
{
    "version": "1.9.4",
    "version_num": 10904,
    "version_desc": "Release notes and changelog",
    "prerelease": false,
    "download_url": "https://github.com/yaerda/dart_simple_live/releases"
}
```

## Installation Instructions

1. Download the IPA file from the release
2. Install using one of these methods:
   - Xcode: Window → Devices and Simulators → Install app
   - 3uTools or similar iOS management tools
   - AltStore for jailbroken devices
3. Trust the developer certificate in iOS Settings → General → VPN & Device Management

## Troubleshooting

### Common Issues:
1. **Build Failures**: Check Flutter version compatibility and dependencies
2. **Signing Issues**: Verify certificate and provisioning profile setup
3. **Installation Issues**: Ensure device is registered in provisioning profile (for signed builds)

### Debugging:
- Check the Actions logs for detailed error information
- Validate `assets/app_version.json` format
- Ensure all required secrets are configured (if using code signing)