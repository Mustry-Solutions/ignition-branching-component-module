# Claude Context for Ignition Branching Module

## Project Overview

This is an Ignition Perspective module that provides a Branching Component for visualizing branching paths. The module is built for **Ignition 8.3+** and requires **Java 17**.

- **Module ID:** `org.mustry.mustryui`
- **Module Name:** Mustry UI
- **Output file:** `build/Mustry_UI.modl`

## Build Instructions

```bash
# Build the module
./gradlew clean build

# The signed module will be at: build/Mustry_UI.modl
```

## Key Configuration Files

| File | Purpose |
|------|---------|
| `build.gradle.kts` | Main module configuration (version, module ID, hooks, scopes) |
| `gradle/libs.versions.toml` | Ignition SDK version and dependencies |
| `gradle/wrapper/gradle-wrapper.properties` | Gradle version (8.5) |
| `gradle.properties` | Module signing configuration |

## Version Configuration

- **Module version:** Set in `build.gradle.kts` under `allprojects { version = "X.Y.Z" }`
- **Ignition SDK version:** Set in `gradle/libs.versions.toml` (currently 8.3.0)
- **Required Ignition version:** Set in `build.gradle.kts` under `requiredIgnitionVersion.set("8.3.0")`

## Project Structure

```
├── common/          # Shared code (component descriptors, schemas)
├── designer/        # Designer hook and UI
├── gateway/         # Gateway hook and component registration
├── web/             # TypeScript/React frontend components
│   └── packages/
│       └── client/  # Perspective client components
└── build/           # Build output (.modl files)
```

## Ignition 8.3 Compatibility Notes

When upgrading from Ignition 8.1 to 8.3:

1. **Java version:** Must use Java 17 (not Java 11)
2. **Gradle plugin:** Use `io.ia.sdk.modl` version 0.4.1+
3. **Gradle version:** Use 8.5+
4. **Free modules:** Do NOT set a license file - causes "No Eula Found" error. Just set `freeModule.set(true)` without `license.set(...)`.

## Module Dependencies

This module depends on the Perspective module:
```kotlin
moduleDependencies.put("com.inductiveautomation.perspective", "DG")
```

## Hooks

- **Gateway:** `org.mustry.gateway.MustryUIGatewayHook`
- **Designer:** `org.mustry.designer.MustryUIDesignerHook`

## Creating a Release

1. Update version in `build.gradle.kts`
2. Run `./gradlew clean build`
3. The signed module is at `build/Mustry_UI.modl`
4. Create GitHub release manually and upload the .modl file

## Testing with Docker

```bash
# Start Ignition 8.3 container
cd docker
docker compose up -d

# View logs
docker logs ignition

# Stop
docker compose down
```

If container name conflict occurs:
```bash
docker rm ignition
```

## Common Issues

- **"Module failed to enable"**: Usually means outdated Gradle plugin (need 0.4.1+)
- **"No Eula Found"**: Remove `license.set(...)` from build.gradle.kts for free modules
- **Java version mismatch**: Ensure all subprojects use `JavaLanguageVersion.of(17)`
