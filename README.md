# Simple WiX Sample

This is a simple sample for a [Windows Installer XML](https://wixtoolset.org) project using minimal authoring to create a Windows Installer package (.msi file).

It contains:

1. **SampleApp** - a simple console application.
2. **SampleProduct** - a WiX project that references the SampleApp project and its target output.

## Authoring

After installing the prerequisites in the [Building](#building) section below:

1. Create a new .NET Core console application (keep project in a separate directory from the solution).
2. Find and create a "Setup Project for WiX v3".
3. Right-click on the setup project and click **Add -> Reference...**
4. Edit *Product.wxs* to change the `ProductName` and `Manufacturer` attributes on the root `Product` element.
5. Edit the `ProductComponents` component group to reference your product file:
   ```xml
    <Component>
      <File Source="$(var.SampleApp.TargetPath)" />
    </Component>
   ```
   You do not need to specify any other attributes.
6. You can optionally author registry values in another component (generally recommended to keep resources separate):
   ```xml
    <Component>
      <RegistryKey Root="HKLM" Key="Software\[Manufacturer]\[ProductName]">
        <RegistryValue Name="InstallDir" Type="string" Value="[INSTALLFOLDER]" />
        <RegistryValue Name="Version" Type="string" Value="[ProductVersion]" KeyPath="yes" />
      </RegistryKey>
    </Component>
   ```
   In this case, we specify the `KeyPath` attribute on a registry value that will change with each update. When multiple resources are contained within a component, you often need to denote which one is the primary one that will help identify when a component has been upgraded.

If you have a lot of projects or project outputs, you can attempt to [harvest those projects](https://wixtoolset.org/documentation/manual/v3/msbuild/target_reference/harvestprojects.html). This option will harvest directories and files output by the projects based on which output groups you select (default: Binaries, Content, and Satellite assemblies). It will also harvest any subdirectories. Please be aware that harvesting a project may not always yield the exact results you want, but may be useful for simple projects, or in early product development releases.

## Building

To build this solution, you'll need:

1. [Microsoft Visual Studio](https://www.visualstudio.com)
2. [Windows Installer XML 3.0](https://wixtoolset.org/releases)
3. [WiX Toolset Extension](https://marketplace.visualstudio.com/search?term=wix&target=VS&sortBy=Relevance) appropriate for your Visual Studio version.

Open the solution and build.

## License

This project is licensed under the MIT license. See [LICENSE.txt](LICENSE.txt) for details.