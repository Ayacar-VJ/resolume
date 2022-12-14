This is the Resolume fork of the FFGL repository. It is up to date and has Visual Studio and Xcode projects to compile 64 bit plugins that can be loaded by Resolume 7.0.3 and up.

The master branch is used for continued development. It will contain the latest features, fixes and bugs. Plugins compiled with the master branch will work in Resolume 7.3.0 and up.
If you do not want to be affected by the latest bugs you can use one of the stable releases. eg FFGL 2.1, which is the most recent released version of the sdk. Plugin development for Resolume 7.0.0/7.0.1/7.0.2 is no longer supported by this repository. These versions are very old and there are many newer versions that users can update to.

You can find some help to get started with FFGL plugin development on the [wiki](https://github.com/resolume/ffgl/wiki).

Also more examples are available on this [repo](https://github.com/flyingrub/ffgl/tree/more/).

## Master branch changes since FFGL 2.1
- Added context state validation in debug builds. This provides plugin developers hints on which context state they need to restore.
- Removed default DllMain implementation so that plugins may implement it without changing the ffgl library.
- File parameters now accept an initial value just like text parameters. (This requires Resolume 7.2 for it to be picked up)
- Added support for grouping parameters together. Set a parameter's group with SetParamGroup, any cosecutive params with the same group will be listed under the same collapsable region (This requires Resolume 7.3.0 for it to be picked up)
- Added support for top-left texture orientation. Hosts that are rendering with the top-left texture orientation currently need to flip both inputs and the output every frame. A plugin can now inform the host that it supports the top-left orientation by setting supportTopLeftTextureOrientation to true. If the host wants to use it then it'll inform the plugin, which can query if it should use top-left or bottom-left using the GetTextureOrientation function. (This requires Resolume 7.3.1 for it to be picked up)
- Added support for hooking into the host's logging system from the plugin, enabling plugin's log messages to be interleaved with the host's messages in the host's log file. (This requires Resolume 7.3.1 for it to be picked up)


## Quickstart

Below are the first steps you need to create and test an FFGL plugin for Resolume. This assumes you have experience with git and C++ development.

### Mac

- Go to `<repo>/build/osx`, open `FFGLPlugins.xcodeproj`
- Create a compilation target for your plugin:
	- Select the Xcode project (top of the tree)
	- Duplicate a target and rename it
	- Remove the old plugin-specific files under Build Phases > Compile Sources (e.g. if you duplicated Gradients, remove `FFGLGradients.cpp`)
	- Duplicating a target in Xcode creates and assigns a new `xx copy-Info.plist` file, but we don't want that. Go to Build Settings > Packaging > Info.plist and change the file name to `FFGLPlugin-Info.plist`.  
	- Find the reference to the newly created `xx copy-Info.plist` file in the Xcode Project Navigator (probably all the way down the panel) and remove it there. When asked, choose Move to Trash.
- In Finder, duplicate a plugin folder and rename the files. Choose a corresponding plugin type, e.g. copy `AddSubtract` if you want to build an Effect plugin or `Gradients` if you want to build a Source plugin.
- Drag the new folder into the Xcode project. You will be asked to which target you want to add them, add them to your new target.
- Go to the target's Build Phases again and make sure there are no resources under the Copy Bundle Resources phase.
- Replace the class names to match your new plugin name and rename the elements in the PluginInfo struct
- Fix up the Build scheme:
	- When duplicating a target, a Build Scheme was also created. Next to the play and stop buttons, click the schemes dropdown and select Manage Schemes. 
	- Rename the scheme that was auto-created (e.g. "Gradient copy")
	- Select it in the scheme drop down.
- Press play (Cmd+B) to compile.
- Copy the resulting `.bundle` file from `<repo>/binaries/debug` to `~/Documents/Resolume/Extra Effects` and start Arena to test it.

### Windows 

This assumes you use Visual Studio 2017

- Go to `<repo>/build/windows`, duplicate a `.vcxproj` and the corresponding `.vcxproj.filters` file, and rename them.
- Open `FFGLPlugins.sln`. Then right-click the Solution in the solution explorer (top of the tree), and choose Add > Existing Project and select the new file.
- Remove the original `.cpp` and `.h` source files from the newly added project, i.e. if you duplicated `Gradient.vcxproj`, remove `FFGLGradients.h` and `FFGLGradients.cpp`
- In Explorer, go to `<repo>/source/`, duplicate a plugin folder and rename the files. Choose a corresponding plugin type, i.e. copy `AddSubtract` if you want to build an Effect plugin or `Gradients` if you want to build a Source plugin.
- Add the new source files to the project by dragging them into Visual Studio, onto your new project.
- If you want to start the build with Visual Studio's Build command (F5), right-click the project and select Set as Startup Project. Altenatively, you can right-click the project and select Build.
- After building, find the resulting `.dll` file in `\binaries\x64\Debug`. Copy it to `<user folder>/Documents/Resolume/Extra Effects`

