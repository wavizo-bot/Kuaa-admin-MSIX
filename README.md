# Kuaa-admin MSIX Package

This repository contains the essential files to build and submit the **kuaa Gerenciador de Provas** (Exam Manager/Admin app) as an MSIX package to the Microsoft Partner Center.

## Package Identity

- **Package/Identity/Name**: `wavizo.KuaaGerenciadodeprovas`
- **Package/Identity/Publisher**: `CN=57BB464E-553F-45B6-A4ED-B253157408EB`
- **Package/Properties/PublisherDisplayName**: `wavizo`
- **Package Family Name (PFN)**: `wavizo.KuaaGerenciadodeprovas_c5p81jb0en0bm`
- **Package SID**: `S-1-15-2-1019287632-1211629304-2026247123-2703169241-4218778349-2026831722-314477589`
- **Store ID**: `9PMTPXXJFDGQ`
- **Version**: `1.2.3.0`

## Structure

```
Kuaa-admin/
├── AppxManifest.xml          # MSIX package manifest
├── Mapping.txt               # File mapping for MakeAppx
├── build-msix.ps1            # Build script
├── assets/                   # MSIX required icons (Square44x44, Square150x150, etc.)
├── icons/                    # App icons (192x192, 512x512, maskable)
├── medals/                   # Achievement medal images
├── index.html                # PWA entry point
├── manifest.webmanifest      # PWA manifest
├── service-worker.js         # Service worker for offline support
├── politica-privacidade.html # Privacy policy (adapted for Gerenciador de Provas)
├── initial-student-package.json # Pre-loaded exam package
└── Host/                     # WebView2 host application
    ├── KuaaAdmin.Host.csproj # .NET 8 project
    ├── Program.cs            # Main entry point
    └── app.manifest          # Application manifest
```

## Prerequisites

- Windows 10/11 with Windows 10 SDK (10.0.17763.0 or later)
- .NET 8 SDK
- Visual Studio 2022 or Build Tools
- Code signing certificate (for Store submission)

## Building the MSIX Package

### Option 1: Using the Build Script (Recommended)

```powershell
# Build for x64 (default)
.\build-msix.ps1

# Build for specific platform
.\build-msix.ps1 -Platform x64
.\build-msix.ps1 -Platform x86
.\build-msix.ps1 -Platform arm64

# Build and sign (requires certificate)
.\build-msix.ps1 -SignPackage -CertificatePath "path\to\cert.pfx" -CertificatePassword "password"
```

### Option 2: Manual Build

1. **Build the host application:**
   ```powershell
   dotnet publish Host\KuaaAdmin.Host.csproj -c Release -r win-x64 --self-contained true -o PackageOutput\Package
   ```

2. **Copy web assets to PackageOutput\Package**

3. **Create MSIX with MakeAppx:**
   ```powershell
   makeappx pack /d PackageOutput\Package /p PackageOutput\wavizo.KuaaGerenciadodeprovas_1.2.3.0_x64.msix /l /o /f Mapping.txt
   ```

4. **Sign the package (for Store):**
   ```powershell
   signtool sign /fd SHA256 /f cert.pfx /p password PackageOutput\wavizo.KuaaGerenciadodeprovas_1.2.3.0_x64.msix
   ```

## Microsoft Partner Center Submission

1. Go to [Partner Center](https://partner.microsoft.com/dashboard)
2. Create a new app submission
3. Reserve the name "kuaa Gerenciador de Provas" (or use existing)
4. Upload the generated `.msix` file
5. Fill in the store listing details
6. Submit for certification

## App Features

- **Exam creation**: Create and manage exam packages for students
- **Question editor**: Visual editor for questions with multiple choice, scoring
- **Package generator**: Export exam packages in kuaa/estudante format
- **Admin preview**: Intermediate panel for reviewing/editing before export
- **Question bank management**: Organize 885 exams, 7,781 questions
- **Bulk import**: Import multiple JSON exam files at once
- **Theme support**: Custom visual themes for the student app
- **Portuguese (Brazil)**: Full localization

## Technical Details

- **Framework**: React 19 + Vite 7 + TypeScript
- **Host**: .NET 8 + WebView2 + Windows App SDK 1.6
- **Capabilities**: internetClient, internetClientServer, privateNetworkClientServer, runFullTrust
- **Target**: Windows 10 1809+ (10.0.17763.0)
- **Architectures**: x86, x64, arm64

## Related Repositories

- **Kuaa-app MSIX**: https://github.com/wavizo-bot/Kuaa-app-MSIX.git
- **Source (Student App)**: https://github.com/wavizo-bot/Kuaa-app
- **Source (Admin Panel)**: https://github.com/wavizo-bot/Kuaa-admin