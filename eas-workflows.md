# EAS Workflows

This document outlines common workflows for using EAS (Expo Application Services) for building and deploying your React Native applications.

## Prerequisites

- EAS CLI installed: `npm install -g eas-cli`
- Logged in to your Expo account: `eas login`
- Project configured with `eas.json`

## Build Workflows

### Development Build

Create a development build for testing:

```bash
# iOS
eas build --profile development --platform ios

# Android
eas build --profile development --platform android

# Both platforms
eas build --profile development --platform all
```

### Preview Build

Create a preview build for internal testing:

```bash
# iOS
eas build --profile preview --platform ios

# Android
eas build --profile preview --platform android
```

### Production Build

Create a production build for app store submission:

```bash
# iOS
eas build --profile production --platform ios

# Android
eas build --profile production --platform android

# Both platforms
eas build --profile production --platform all
```

## Submit Workflows

### Submit to App Stores

```bash
# Submit to Apple App Store
eas submit --platform ios

# Submit to Google Play Store
eas submit --platform android

# Submit with specific build ID
eas submit --platform ios --id [BUILD_ID]
```

## Update Workflows

### OTA Updates

Publish over-the-air updates using EAS Update:

```bash
# Publish to default branch
eas update --branch production --message "Bug fixes and improvements"

# Publish to specific channel
eas update --branch preview --message "Testing new features"

# Publish to multiple branches
eas update --branch production,staging --message "Critical hotfix"
```

### View Updates

```bash
# List updates
eas update:list

# View update details
eas update:view [UPDATE_ID]

# Delete an update
eas update:delete [UPDATE_ID]
```

## Branch Management

```bash
# List branches
eas branch:list

# Create a new branch
eas branch:create [BRANCH_NAME]

# Delete a branch
eas branch:delete [BRANCH_NAME]

# View branch details
eas branch:view [BRANCH_NAME]
```

## Channel Management

```bash
# List channels
eas channel:list

# Create a channel
eas channel:create [CHANNEL_NAME]

# Point channel to a branch
eas channel:edit [CHANNEL_NAME] --branch [BRANCH_NAME]

# View channel details
eas channel:view [CHANNEL_NAME]
```

## Device Management

```bash
# Register iOS device for development builds
eas device:create

# List registered devices
eas device:list

# Delete a device
eas device:delete [DEVICE_ID]
```

## Credentials Management

```bash
# View credentials
eas credentials

# Configure iOS credentials
eas credentials --platform ios

# Configure Android credentials
eas credentials --platform android
```

## Common Workflows

### Local Development with EAS Update

1. Run development build on device
2. Make code changes
3. Publish update: `eas update --branch development --message "Feature updates"`
4. App automatically receives update

### Release Process

1. Test on preview build
2. Create production build: `eas build --profile production --platform all`
3. Test production build internally
4. Submit to stores: `eas submit --platform all`
5. Monitor rollout

### Hotfix Process

1. Create fix in codebase
2. Publish OTA update: `eas update --branch production --message "Critical hotfix"`
3. Users receive update without app store submission

## Build Monitoring

```bash
# List builds
eas build:list

# View build details
eas build:view [BUILD_ID]

# Cancel a build
eas build:cancel [BUILD_ID]

# Inspect build
eas build:inspect [BUILD_ID]
```

## Webhooks

Set up webhooks for build and submit events:

```bash
# Create webhook
eas webhook:create --url [WEBHOOK_URL] --event BUILD --event SUBMIT

# List webhooks
eas webhook:list

# Delete webhook
eas webhook:delete [WEBHOOK_ID]
```

## Environment Variables

```bash
# Create secret
eas secret:create --name API_KEY --value [VALUE] --type string

# List secrets
eas secret:list

# Delete secret
eas secret:delete --name API_KEY
```

## Best Practices

1. **Use build profiles** - Separate development, preview, and production configurations
2. **Version management** - Increment version numbers appropriately
3. **Branch strategy** - Use branches for different environments (production, staging, development)
4. **Channel strategy** - Map channels to your deployment stages
5. **Test before submit** - Always test production builds before app store submission
6. **OTA updates** - Use for quick fixes, but don't abuse (keep bundle size in mind)
7. **Credentials backup** - Keep credentials backed up securely
8. **Monitor builds** - Set up webhooks or check build status regularly

## Troubleshooting

### Build fails
- Check build logs: `eas build:view [BUILD_ID]`
- Verify credentials are up to date
- Check for platform-specific issues

### Update not received
- Verify device is on correct channel
- Check update compatibility with build
- Force close and reopen app

### Submit fails
- Verify build is production profile
- Check app store credentials
- Review app store requirements
