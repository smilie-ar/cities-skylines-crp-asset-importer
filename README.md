# cities-skylines-crp-asset-importer
A basic GUI application to export .crp models from Cities Skylines 2 and extract meshes in .obj format.

# TL;DR: CRP to OBJ Converter

A drag-and-drop tool to convert Cities Skylines `.crp` (Colossal Raw Asset Package) files to OBJ format with textures.

## Requirements

- **Windows** (required for GUI)
- **.NET 8.0 Runtime** (download from https://dotnet.microsoft.com/download/dotnet/8.0)

## Installation

1. Install download the .zip file from Releases and extract to a directory.
2. Copy all files from the `publish` folder to a location of your choice
3. Make sure `.NET 8.0 Runtime` is installed
4. Run `CRPConverter.exe`

## Usage

1. **Select CRP File**: Drag and drop a `.crp` file onto the application window, or click to browse
2. **Select Output Folder**: Click "Browse..." to choose where to save the exported files
3. **Convert**: Click "Convert" to start the conversion

## Output

Files are exported to a subfolder named after the CRP file:

```
OutputFolder/
└── [AssetName]/
    ├── mesh_000_obj_...obj      # 3D mesh (OBJ format)
    ├── mesh_000_obj_...mtl      # Material file
    ├── mesh_000_obj_...png       # Texture (PNG)
    ├── texture_001_...png        # Extracted texture
    ├── raw_...bin                # Unexported raw data
    └── ...
```

## Features

- **Mesh Export**: OBJ format with MTL material references
- **Texture Export**: PNG (lossless) and JPEG (compressed)
- **Progress Tracking**: Real-time progress bar and log
- **Error Handling**: Skips problematic assets and logs errors
- **Content Detection**: Automatically detects Unity mesh/texture/material content

## Usage Tips

- When importing into Blender/Unreal, sort the exported `.obj` files by file size
- Import the **largest/largest file size** OBJ - this is usually LOD0 (highest detail)
- LOD1 and LOD2 are lower detail versions and will look worse
- For road signs and lamp posts, the largest mesh is typically the one you want

## CRP File Locations

### Workshop Assets
```
C:\Program Files (x86)\Steam\steamapps\workshop\content\255710\
```

### Local Assets
```
%LocalAppData%\Colossal Order\Cities_Skylines\Addons\Assets\
```

### Vanilla Game Assets
```
<Steam Library>\steamapps\common\Cities_Skylines\Cities_Data\
```

## Supported Asset Types

| Type ID | Content Type | Export Status |
|---------|-------------|---------------|
| 4 | UnityEngine.Mesh | OBJ export |
| 3 | UnityEngine.Texture2D | PNG export |
| 2 | UnityEngine.Material | Parsed |
| 1 | UnityEngine.GameObject | Raw |
| 103 | CustomAsset | Raw |
| 80 | Locale | Raw |

## Known Limitations

- ⚠️ **UV Coordinates**: May not export correctly - textures may not map to meshes properly
- ⚠️ **Texture Mapping**: In most cases, textures will not apply correctly in 3D software (Blender, etc.)
- ⚠️ **DDS Decompression**: Some DDS textures may not convert to PNG properly
- Some meshes may fail to parse if they're in an unsupported Unity format
- Roads and signs typically have good mesh data for extraction
- Raw binary files may be saved for unexported assets

## Troubleshooting

**"0 exporting" errors**
- Ensure you're using a valid Cities Skylines .crp file

**"Failed to convert mesh" errors**
- The mesh may use a Unity format not yet supported
- Try using AssetStudio (https://github.com/Razviar/assetstudio) as an alternative

## Build from Source

```bash
dotnet restore CRPConverter.sln
dotnet build CRPConverter.sln -c Release
dotnet publish src/CRPConverter.GUI/CRPConverter.GUI.csproj -c Release -r win-x64 -o publish
```

## License

This tool is provided as-is for personal use with Cities Skylines assets.

