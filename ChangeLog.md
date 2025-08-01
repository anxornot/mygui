## MyGUI v3.4.4

### Widgets
- Fixed TabItem child widgets not being clickable in some cases (regression introduced in v3.4.3)
- Fixed MenuControl popup positioning issue when it is unable to fit into screen height
- Guard against division by zero in ScrollBar::TrackMove

### Resources
- ResourceTrueTypeFont: preserve explicit 0.0 advance in ResourceManualFont::deserialization

### Platforms
- OpenGL platforms: removed glew dependency

### Other
- Fixed build with MYGUI_USE_FREETYPE=OFF
- Fixed build with static Ogre
- CMake fixes

## MyGUI v3.4.3

### Core
- Allow passing lambdas into MyGUI::newDelegate without explicitly wrapping them into std::function

### Widgets
- Add missing eventClipboardChanged when trying to cut/copy with no text selected
- Fixed rare crash when changing widget style after it was clicked in the same frame
- Fixed TabItem left disabled when rapidly switching between tabs
- Improve performance of the TabControl tabs switching
- Fixed ignoring "FontHeight" property when "FontName" is also set
- Move MyGUI::DEFAULT to deprecated, add TabControl::DEFAULT_WIDTH instead

### Platforms
- Fix build with Ogre 14

### Tools
- Minor fixes for macOS
- Use macOS Command key in tools

### Other
- Switch to C++17
- Use string_view, unique_ptr and some other stuff
- Various code improvements, including range base for migration and some other minor stuff
- Add some missing MYGUI_DONT_USE_OBSOLETE checks for deprecated stuff
- Remove MYGUI_DEBUG_MODE from headers to avoid issues with mixed debug/release builds

## MyGUI v3.4.2
### Core
- Add missing MYGUI_EXPORT for ControllerRepeatClick (was not linking in non-static builds)
- Use commands for key combinations on EditBox, provide macOS proper bindings
- Add support for string_view in UString

### Widgets
- ComboBox and ListBox: make "AddItem" use LanguageManager::replaceTags same way it is done for caption properties
- EditBox: Allow text input while holding Ctrl (Allow key combinations involving CTRL that produce printable characters)

### Platforms
- OpenGL: Fix build with libX11-free wayland setup
- Ogre: Correct vertex color order for ogre 13

### Demos and Tools
- Use SDL_WINDOWEVENT_FOCUS_GAINED/SDL_WINDOWEVENT_FOCUS_LOST events for better detection when window should have low FPS
- Fix macOS resources paths, do not use MACOSX_BUNDLE
- Fix various deprecated or incorrect resources usage

### Other
- Add missing MYGUI_EXPORT to fix C4275 warnings
- Various minor build fixes and improvements
- Enable more warnings in MYGUI_HIGH_LEVEL_WARNINGS mode
- Use GNUInstallDirs to install includes

## MyGUI v3.4.1
### Core
- Implement new MSDF font, that lets use render huge fonts without having big textures, also allows using only one texture for all sizes (see Demo_MsdfFont)
- Add new eventWidgetDestroyed
- Add support for custom shaders for each render item, used for subskin (so far used only for MSDF fonts)
- Add support for Unicode code points outside of the Basic Multilingual Plane (code points above 2^16, represented by two UTF-16 code units)
- Allow std::function in the MyGUI::newDelegate
- Allow const functions and const classes in the MyGUI::newDelegate
- Make most getters const
- Fix UB in RotatingSkin::_rebuildGeometry when all vertices are cropped
- Fix eventMouseButtonClick being triggered even when mouse is pressed not over widget
- Fix InputManager::injectMouseMove returning true when mouse is pressed, but there is no focused widget
- Fix bug with Popup widget being picked by mouse, when it is invisible because of invisible parent
- Fix Widget::setDepth call making Overlapped child widgets not visible

### Widgets
- ListBox: fix getWidgetByIndex when not all widgets are visible
- EditBox: fix getFontHeight always returning 0
- EditBox: fix rare bug with text selection when holding mouse and changing text
- EditBox: fix scroll position when selecting text below
- Button: make Button without "normal_checked" skin display "pushed" skin (fixes button visual state when setStateSelected(true) is called)
- Window: add borders snapping, when resizing it (previously only move was snapping borders)

### Platforms
- Ogre:
  * make it possible to use OGRE_RESOURCEMANAGER_STRICT=1 mode (always specify resource group)
  * other minor performance improvements
  * use Ogre::Renderable to render Gui, this fixes several issues with recent Ogre version
  * fix RTT render with RenderSystem_DirectX11
- All: make all interfaces const-correct. Warning: custom render platforms outside of MyGUI repo must be updated to match new virtual functions API

### Demos and Tools
- Add new SdlBaseManager, that is used for all platforms base code instead of multiple BaseManager-s with duplicate code
- Remove no longer needed and no longer used OIS, Win32 and Win32+OIS input systems only SDL2 can is used in demos now

### Tools
- LayoutEditor: fix multi language support

### Demos
- New Demo_MsdfFont
- Make all demos with 3d render work with the latest Ogre

### Other
- Use GNUInstallDirs to determine where libraries should be installed
- Use precompiled headers from CMake 3.16, enable precompiled headers on all target platforms
- Remove many outdated or not used CMake options and scripts

## MyGUI v3.4.0
### Core
- Added LanguageManager::getLanguages
- Fix lost key focus event not being triggered sometimes
- Fix inconsistency: _riseMouseButtonMove was using Layer coordinates (most of the time). _riseMouseButtonReleased/Pressed were using absolute coordinates.
- Other InputManager events fixes.
- Minor MacOS build fixes.
- Cleaner and much faster logic for adding and removing render items (it was the performance bottleneck when working with many items on screen).

### Widgets
- ScrollBar: Fixed wrong tracking area bug.
- Button: make Button with key focus (can be set only manually through code) have highlighted state.
- ItemBox: add possibility to not update ItemBox content when working with items (huge speed boost, when adding/removing many in single frame)
- EditText: add support for alpha in text and text shadow colours (multiplied with widget's alpha).
- ItemBox: add options to hide ItemBox scroll bars (same as for ScrollView and EditBox).
- MultiListBox: slightly advanced and more flexible custom compare function.
- EditBox: add Ctrl+Left/Right hotkey support (moves cursor to next word).
- MultiListBox: add eventNotifyItem.

### Platforms
- OpenGL3Platform: Added new rendering platform.
- OpenGLESPlatform: Added new rendering platform.
- Emscripten support (works for Ogre and all OpenGL platforms).
- OgrePlatform: Many minor build-related fixes and imrovements, including newer Ogre support and better CMake scripts.
- DirectX 11 and OpenGL3: Fixed application window resizing in editors and demos.
- All platforms: Fix possible stack overflow in ***DataManager::getDataPath.
- All platforms: better VertexBuffer reallocation strategy for all platforms (in ::lock call, not in ::setVertexCount).
- All platforms: Various minor fixes and code cleanup.

### Resources
- ResourceManualFont: Added setTexture.
- ResourceTrueTypeFont: Optimize TrueTupeFont render speed (half getGlyphInfo search time).
- ResourceTrueTypeFont: Remove no longer needed freetype bytecode bug fix.

### Tools
- LayoutEditor: Fixed "[DEFAULT]" value not being handles properly (was colored white in ComboBox, was saved in layout).
- LayoutEditor: Other minor fixes and improvements.

### Demos
- Added SDL2 support.

### Other
- Switched to C++11: various related code improvements and cleanups.

## MyGUI v3.2.2
### Core
- Fixed eventChangeMouseFocus and eventChangeKeyFocus not being triggered in certain cases.

### Widgets
- ComboBox: Fixed mouse events incorrectly going to the widget below the ComboBox when clicking on an item.
- ScrollBar: Added button repeat support, ScrollBar buttons will trigger repeatedly so long as the mouse is held down.

### Platforms
- OpenGLPlatform: Added option to use system GLEW library.
- OgrePlatform: Improved performance of OgreDataManager::isDataExist.
- OgrePlatform: Fixed build failure regarding boost libraries used by Ogre not being found.

### Resources
- ResourceManualFont: Added API to create a ResourceManualFont in code rather than from XML.

### Tools
- Fixed broken file dialog directory listing on Linux and make sure files are sorted.

## MyGUI v3.2.1
### Core
- Added events to clipboard manager: eventClipboardRequested, eventClipboardChanged.
- Added API access to child LayerNodes. 
- Added pointer to ControllerItem in all controller events.
- Key focus will be reset when the key focus widget is hidden.
- Fixed incorrectly detecting double click when clicking on different widgets.
- Fixed a memory leak when creating or changing widgets in several cases.
- Fixed a lot of warnings from cppcheck.
- Other minor fixes.
- Remove possibility to compile without std::typeinfo (no longer needed for android, MYGUI_RTTI_DISABLE_TYPE_INFO option removed).
- Remove -msse from compiler flags.

### Widgets
- Added Depth property for child widgets, that can be used for rendering order.
- Added eventChangeCoord event.
- Fixed Enabled property not being inherited from parent widgets.
- ListBox: Added eventNotifyItem for key or mouse press on item widgets.
- ListBox: Added getWidgetByIndex public API to allow item widget access from outside.
- EditBox: Fixed several keyboard shortcuts (such as copy & paste) not working when key and text events are injected separately by the user.
- ImageBox: Added getImageSize method.
- ScrollBar: Added WheelPage property for mouse wheel scroll step.

### SubWidgets
- PolygonalSkin: Added stroke support for drawing dashed lines (can be seen in UnitTest_Spline).
- RotatingSkin: Added properties to skin XML.

### Resources
- ResourceTrueTypeFont: Added Distance parameter for margin between glyphs in generated font texture.
- ResourceImageSet, ResourceTrueTypeFont: Added public API to allow creating in code rather than from XML.
- ResourceManualFont: Added support to set glyph size separately rather than deriving it from image coordinates.

### Skins
- Added Dark skin (used by default in Tools).
- Added Black skin.
- Minor updates to other skins.

### Tools
- Significant improvements to LayoutEditor, FontEditor, ImageEditor and SkinEditor.

### Demos
- Minor updates to several demos.

### UnitTests
- Added DataInfoTest.

### Platforms
- Added Dummy platform.
- Various fixes for DirectX11 platform.
- Slightly refactored OpenGL platform.
- Added DataMemoryStream class.

### Other
- Updated C# wrapper.
- Switched from LGPL to MIT license.
- Documentation improvements.

## MyGUI v3.2.0
### Core
- MyGUI can be compiled without std::typeinfo.
- Fixed an alignment bug on non-screen layers (for example on the RTT layer).
- Fixed bug with widgets on same layer changing their draw order.

### Widgets
- DDContainer: Added a resetDrag method that stops a drag operation programmatically.
- TextBox: Added optional text shadow.
- EditBox: Fixed incorrect text display when the text area was lower than the font height.
- EditBox: Reset Redo changes when text was changed.
- Canvas: Fixed incorrect destruction.
- ProgressBar: Fixed a problem that caused the progress value to be displayed incorrectly after the widget was resized.

### SubWidgets
- RotatingSkin: Fixed incorrect display due to swapped UV coordinates.

### Resources
- ResourceTrueTypeFont:
   * Decreased texture memory usage (typically by 50% or more).
   * Improved startup and runtime performance.
   * Added support for Windows FON/FNT bitmap fonts and for embedded SBIT bitmaps in TrueType fonts.
   * Added support for enhanced glyph metrics, typographically correct line spacing, and overlapping glyphs; e.g., "fj".
   * Implemented the ability to specify the code point used as a substitute for missing code points (now defaults to the "Not Defined" glyph instead of "Space").
   * Implemented subpixel positioning.
   * Added useful default values for several properties.
   * Deprecated several properties that are no longer useful.
   * Fixed problems that made it impossible to use whitespace characters other than "Space" and "Tab".
   * Fixed several crashes and memory leaks.
   * Replaced an assertion with a log message when a font cannot be loaded.
- ResourceManualFont:
   * Added support for enhanced glyph metrics and overlapping glyphs; e.g., "fj".
   * Implemented the ability to specify the code point used as a substitute for missing code points.

### Wraps
- BaseLayout works properly when it's completely empty.

### Plugins
- Updated the BerkeliumWidget plugin.

### Tools
- Various fixes in LayoutEditor.
- FontViewer:
   * Added support for all of the new features in ResourceTrueTypeFont and ResourceManualFont.
   * Added support for additional font types: *.ttc, *.otf, *.pfa, *.fon, and *.fnt.
   * Redesigned the user interface.
   * Implemented automatic generation of the output filenames when saving.
   * Fixed several crashes and memory leaks.

### Platforms
- Keep solid mode when Ogre is in wireframe.
- Fixed a few bugs in the OpenGL platform (buffers not cleaned up).
- New DirectX11 platform prototype.

## MyGUI v3.2.0 RC1
### Core
- Skin properties were simplified for more convenient creation in the editor.
- All widgets events is now multidelegates.
- Support of right mouse button drag.
- If MyGUI is built from svn revision log file also contain svn revision (in addition to version).
- Fixed bug with texture creation for fonts.
- Other minor additions and bug-fixes.

### Managers
- LanguageManager now replace tags recursively.
- Fixed bug in LanguageManager when missing language was selected.

### Resources
- Add Layout resource, layouts loaded and parsed only once at first use.
- All resource files now have "MyGUI_" prefix to avoid naming conflicts.

### Widgets
- VScroll and HScroll replaced with single ScrollBar class.
- Edit renamed into EditBox.
- List renamed into ListBox.
- MenuCtrl renamed into MenuControl.
- MultiList renamed into MultiListBox.
- Progress renamed into ProgressBar.
- StaticImage renamed into ImageBox.
- StaticText renamed into TextBox.
- Tab renamed into TabControl.
- New Window property Moveable.
- Message moved from Core to Common.
- MultiListBox now support different modes of columns resizing.
- Button properties StateCheck and ButtonPressed replaced with single property StateSelected.
- ListBox and ComboBox now support ToolTip.
- Menu items can be checked now, tick icon appears.
- Property AlignVert renamed into VerticalAlignment.
- Hidden widget lose input and focus.
- Active elements visualisation in list now properly respond to changes in the list.
- All text related properties was moved from Widget to TextBox.

### SubWidgets
- All widgets now have Colour property that colours whole widget.
- RawRect subwidget was removed.
- Implemented PolygonalSkin sub widget for lines and splines drawing.

### Tools
- SkinEditor added.
- All skins created in SkinEditor.
- LayoutEditor now support projects creation.
- Possibility to create MultiListBox columns in LayoutEditor.
- Possibility to create PopupMenu\MenuBar in LayoutEditor.
- Colour selection window added in LayoutEditor.
- Show skin preview in LayoutEditor.
- Scaling support in LayoutEditor.
- Possibility to change widget's type in LayoutEditor.
- Possibility to add custom properties in LayoutEditor.
- Removed check for unique name in LayoutEditor (not required any more).
- FontViewer now same generated font in two formats: as ttf + xml that use freetype in runtime for texture generation and as manual font (texture and glyphs description)

### Platforms
- BasisManager for OpenGL and DirectX now properly display Win32 window content without bugs with offset.
- Fixed memory leak in OpenGL's and DirectX's DataManager.
- Possibility to save texture in OpenGL.
- Fix typo in names vertext -> vertex.

### Plugins
- Awesomium plugin removed (not LGPL anymore).

### UnitTests
- New UnitTest for PolygonalSkin.
- New UnitTest for RotatingSkin.

### CMake
- New MYGUI_CHECK_MEMORY_LEAKS option for memory leaks detection in debug mode (MSVC only).
- New MYGUI_USE_PROJECT_FOLDERS option for project folders inside generated solution (MSVC only).
- UnitTest and Demos options were renames.
- Win32 input used by default in demos/tools under windows instead of OIS (OIS input still used in other platforms).

## MyGUI v3.0.1
### License
- in LGPL added exclusion for static linking for possibility to use MyGUI in apps for iPhone
- all Media except some files created not by MyGUI dev tean is licensed under MIT now

### Core
- Refactoring to reduce the possbility of bugs (based on info from static analyzers).

### Renders
- Support of textures created inside Ogre (wrapping of existing texture)

### Widget
- added getCaptionWidget to Window: retun pointer to Window caption for text control (fonts and it's size)
- RenderBoxScene fixed
- fixed bug with Disabled widgets that was rendered as not disabled

### Managers
- added log message if skin used for widget wasn't found.

### Utility
- fixed loading on non-existing textures for widgets picking

### Wraps
- New option to generate wrappers with CMake

### Plugins
- optional AwesomiumPlugin and HikariPlugin option added to CMake config

### Resources
- fixed bug with incorrect font rendering on some old videocards : TrueType font now rendered into R8G8B8A8 on videocards that doesn't support L8A8 (OgreRender)

## MyGUI v3.0.0
### Core
- rendering separated from core

### Widgets
- removed widget RenderBox - moved into Common as wrapper
- new widget ListCtrl - base widget for lists with OwnDraw, horizontal scroll and lines with different height support
- new widget ListBox - list implementation with horizontal scroll (based on ListCtrl)

### Managers
- new FactoryManager - manager creates instances of objects that implements IObject (resources, widgets, controllers, etc.)
- new DataManager - possibility to create your own type or data source

### Controllers
- controllers support deserialization, possibility to create through XML

### Resources
- new resource type ImageSetPointer for creating cursors based on ImageSet with animation support
- new resource type ManualPointer for creating cursors based on texture
- new resource type TrueTypeFont for creating true type fonts
- new resource type ManualFont for creating fonts based on texture
- new resource type Skin for skin description

### SubWidgets
- new SubWidget RotateSkin with rotating geometry support

### Tools
- improved LayoutEditor, controllers support

### Demos
- Demo_Pointers - demonstration of cursor contexts and W32 cursors
- Demo_RenderBox - demonstration of RenderBox usage (camera view inside widget, model view inside widget)

### Plugins
- Plugin_AwesomiumWidget - sample plugin implementation for Awesomium (html inside widget)
- Plugin_HikariWidget - sample plugin implementation for Hikari (Flash inside widget)

### UnitTests
- UnitTest_Layers - sample of using GUI inside 3D scene
- UnitTest_RTTLayers - sample of rendering Layer into texture
- UnitTest_TextureAnimation - sample of RTT node, for caching or for animating whole window
- UnitTest_TreeControl - sample implementation of tree control (author : Pavel Turin)

## MyGUI v2.2.3
### SubWidgets 
- subwidget EditText was reimplemented
   * word wrap support in edit mode
   * cursor and text selecting with ManualFont

### Widget
- fixed problem with inherited Disabled and Visible conditions

## MyGUI v2.2.2
### Core
- widget hierarchy changed - style added:
   * Child - child widget, cropped by parent widget borders, no overlapping (used by default for child widgets)
   * Popup - popup widget, have parent widget, but not cropped on its borders
   * Overlapped - child widget, cropped by parent widget borders, can overlap (used by default for root widgets)
- support of detaching and attaching of widgets, possibility to change skin or style in runtime
- added fast RTTI

### Widgets
- new widget DDContainer - base class for all widgets that have drag'n'drop
- new widget MenuCtrl - base class for all menus (such as MenuBar or PopupMenu)
- new widget MenuBar
- new widget ScrllView
- Widgets tool-tip support
- StaticImage animation support

### Managers
- new ResourceManager - allows to load user resources (for example ResourceImageSet)
- new LanguageManager - allows to replace tags in layouts with strings loaded from language files, manual creating and replacing tags
- PointerManager - support animated pointers
- FontManager - support of loading custom fonts from texture

### Controllers
- ControllerPosition improved, Jump move mode added

### Resources
- new resource type ResourceImageSet - used for group of images, with animation, loads through XML

### SubWidgets
- new subwidget RawRect for direct access to quad vertices
- new subwidget TileRect for tiling

### Others
- all widgets-containers have Any (like boost::any) for storing user info about item (for example ComboBox items, Menu items)
- configuration files versions in XML
- two new themes for skin

### Wrappers
- BaseLayout - base class for layout wrapper
- BaseItemBox - base class for ItemBox wrapper
- added PanelView - made using wrappers

 
### Tools
- LayoutEditor - external resources support, several interface improvements
- added ImageSetViewer for ResourceImageSet viewing

### Demos
- Demo_Colour - colour selection control, RawRect subwidget and colouring in text
- Demo_Console - simple console implementation for changing some widget properties
- Demo_Controllers - ControllerFadeAlpha and ControllerPosition examples
- Demo_Font - true-type font generation with possibility to save into MyGUI font configuration file
- Demo_Gui - demonstration of using different widgets and styles (something like Ogre Demo_Gui)
- Demo_ItemBox - ItemBox, BaseItemBox wrapper, ToolTip, external user resource loaded through XML
- Demo_PanelView - PanelView control implementation
- Demo_Picking - widget with mouse picking creation
- Demo_PluginStrangeButton - plugin an user widget
- Demo_ScrollView - ScrollView examples
- Demo_Themes - new MyGUI themes for skin
- Demo_StaticImage - simple ImageSet viewer implementation
