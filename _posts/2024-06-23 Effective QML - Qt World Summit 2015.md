---
title: Effective QML - Qt World Summit 2015
categories: Programming Language
tags:
  - qt
  - 开发语言
  - qml
  - qtquick
  - cmake
date: 2024-06-23 18:39:50
---

[QtWS15- Effective QML, Thomas McGuire, KDAB](https://www.youtube.com/watch?v=vzs5VPTf4QQ)

## Tooling
### Tip #1 Know your profiling tools
- QML Profiler
- `QST_RENDER_TIMING`
- `QSG_VISUALIZE=[batches,clip,changes,overdraw]`
```qml
import QtQuick 2.0 as QQ2
import SlideViewer 1.0
import Cpp 1.0

SlideDeck {
	id: root
	template: "QtWS2015Template"

	property var config: QQ2.QtObject {
		property string titile: "Effective QML
		property string kdabian: "Thomas McGuire"
		property string jobTitle: "Senior Software Engineer"
		property string email: "thomas.mcguire@kdab.com"

		property string student: "" // Need to shut up private copy label
	}

	Slide {
		id: slide1
		slideid: 1
		title: "Effective?"

		centered: QQ2.Item {
			width: slide1.width
			QQ2.Text {
				id: quote
				anchors.centerIn: parent
				width: parent.width
				horizontalAlignment: QQ2.Text.AlignHCenter
				verticalAlignment: QQ2.Text.AlignVCenter
				...
			}
		}
	}

	Slide {
		id: shortcutF5slide
		slideId: 7

		Text {
			text: "* F5 for instant reload
				   ** Quicker edit and run cycle
				   ** Combine with @iCls{QFileSystemWatcher} for even quicker cycle"
		}

		CppCode {
			visible: shortcutF5slide.minStep(1)
			code:  "class UltraView : public QQuickView {
						   ...
					protected:
						void keyPressEvent(QKeyEvent *event) override {
							if (event->key() == Qt::Key_F5) {
								auto oldSource = source();
								setSource({});
								engine()->clearComponentCache();
								setSource(oldSource);
							}
						}
					};"
		}
		...
	}
}
```
### Tip #2 Add shortcuts for increased productivity
- F5 for instand reload
	- Quicker edit and run cycle
	- Combine with `QFileSystemWatcher for even quicker cycle
- Caveats
	- Does not work when QML files in resource file
	- Sometimes crashes complex applications
	- State stored in QML lost (see tip #13)
- See Pelagicore's *qmllive* for a more comprehensive solution
- F10 for slowing down animations
```cpp
#include <private/qabstractanimation_p.h>

...

else if (event->key() == Qt::Key_F10) {
	QUnifiedTimer::instance()->setSlowModeEnabled(true);
	QUnifiedTimer::instance()->setSlowDownFactor(10);
} else if (event->key() == Qt::Key_F11) {
	QUnifiedTimer::instance()->setSlowModeEnabled(false);
	QUnifiedTimer::instance()->setSlowDownFactor(1);
}
```
### Tip #3 Integrate qmllint into your build system
- Available since Qt 5.4
- Remember to integrate into CLI
- Catches syntax errors only
```cmake
set(QML_FILES Button.qml Rectangle.qml)
add_custom_target(lint)
foreach(QML_FILE ${QML_FILES})
	add_custom_command(TARGET lint COMMAND qmllint ${QML_FILE}
					   COMMENT "Linting file ${QML_FILE}")
endforeach()
...
add_executable(myapp main.cpp ${QML_FILE})
```text
# make lint
Linting file /home/thomas/[..]/qml/Button.qml
Linting file /home/thomas/[..]/qml/Settings.qml
/home/thomas/[..]/qml/Settings.qml:5 : Syntax error
CMakeFiles/lint.dir/build.make:52: recipe for target 'lint' failed
make[3]: *** [lint] Error 255
```
### Tip #4 Use GammaRay for debugging 
- www.kdab.com/gammaray/
- Items, scene grpah, focus, visibility, layouting...
### Tip #5 Use available debugging environment variables
- `QV4_FORCE_INTERPRETER=1`
	- Use bytecode backend instead of JIT for JS code
	- Better backtraces
	- Automatic when started from within QtCreator
- `QML_IMPORT_TRACE=1`
	- For tracking down module import problems
- Find more by grepping for `DEFINE_BOOL_CONFIG_OPTION` and `getenv`
```cmake
export QSG_SANITY_CHECK=1
export QML_CHECK_TYPES=1 # Crashes
export QML_PARENT_TEST=1
export QML_LEAK_CHECK=1
```
`auto leak = new QQuickItem(nullptr);`
```text
scenegraph/util/qsgtexture.cpp:115: Number of leaked textures: 0
scenegraph/coreapi/qsgmaterial.cpp:549: Number of leaked materials: 0
scenegraph/coreapi/qsgnode.cpp:50: Number of leaked nodes: 0
items/qquickitem.cpp:2215: Number of leaked items: 1
```
- `qputenv()` does not work. Qt calls `qgetenv()` in static initialization
## Understanding
### Tip #6 Read the docs
### Tip #7 Be aware of object ownership
- `QObjects` owned by QML garbage-collected
- `QObjects` owned by C++ deleted by parent or manually
`Q_INVOKABLE QObject* firstSong() { return m_songs[0]; }`
```qml
property QtObject currentSong;
...
onClicked: currentSong = musicPlayer.firstSong();
```
- Ownership of `QObjects` handed from C++ to QML determined heuristically
	- Use `QQmlEngine::setObjectOwnership()` for explicit control
### Tip #8 Know the difference between parent and parentItem
- `QObject::parent`
	- defines object tree, deletes children automatically
- `QQuickItem::parentItem`
	- defines visual tree
	- provides parent(!) property for QML
- Not always the same
	- `Repeater` does not set `QObject::parent`
- Pay attention when you modify `parent` or `parentItem`!
### Tip #9 Know what QQuickItem methods get called
- Loading QML file
	1. Compiling
	2. Creating: Constructor, `QQuickItem::classBegin()`
	3. Binding evaluation: Property Setters
	4. Completion: `QQuickItem::componentComplete()`
- Rendering
	1. Polish: `QQuickItem::updatePolish()`
	2. Synchronizing: `QQuickItem::updatePaintNode()`
	3. Rendering
## Coding
### Tip #10 Layer QML on top of C++
```cpp
QObject *artistLabel = rootItem->findChildren("artistLabel");
artistLabel->setProperty("text", m_currentSong->artist);
...
QObject *nextLabel = rootItem->findChildren("nextLabel");
connect(nextButton, SIGNAL(clicked()), this, &MusicPlayer::nextSong);

QObject *musicPlayer = nextButton->parent();
QMetaObject::invodeMethod(musicPlayer, "startAnimation",
						  Q_ARG(QVariant, duration));
```
```qml
Text {
	text: musicPlayer.currentSong.artist
}

Button {
	onClicked: musicPlayer.nextSong()
}

Slider {
	value: musicPlayer.volume
}
```
- Easier to understand and maintain
- QML can be changed without breaking C++ layer
	- Refactoring
	- Different QML files for different screen sizes or OSes
- Use QML as a UI layer only
### Tip #11 Be declarative
```cpp
class MusicPlayer : public QObject {
public:
	Q_INVOKABLE void play();
	Q_INVOKABLE void pause();
}
```
```qml
Button {
	icon: "play.png"
	onClicked: {
		if (icon == "play.png") {
			musicPlayer.play();
			icon = "pause.png"
		} else {
			musicPlayer.pause()
			icon = "play.png"
		}
	}
}
```
```cpp
class MusicPlayer : public QObject {
	Q_PROPERTY(bool playing READ playing WRITE setPlaying NOTIFY playingChanged)
public:
	void setPlaying(bool playing) {
		if (m_playing != playing) {
			m_playing = playing;
			if (playing)
				play();
			else
				pause();
			emit playingChanged();
		}
	}
}
```
```qml
Button {
	icon: musicPlayer.playing ? "pause.png" : "play.png"
	onClicked: musicPlayer.playing = !musicPlayer.playing
}
```
- Avoid imperative code in QML
	- let bindings do the work automatically
	- code smell: assignment outside of input signal handlers like `onClicked`
	- code stink: on *Property*Changed signal handlers
- Expose declarative APIs from C++
	- properties instead of `Q_INVOKABLE`s and slots
	- declarative concepts like in `Repeater` and `Behaviour`
### Tip #12 Avoid JS code
- Move code to C++ if possible
	- faster
	- more maintainable
- Treat JS code as a code smell
	- Exception: bindings, some signal handlers
- Following tip #11 helps!
### Tip #13 Avoid storing non-UI state in QML
```cpp
class MusicPlayer : public QObject {
public:
	Q_INVOKABLE void setVolume(int volume) {
		m_backend->setVolume(volume);
	}
}
```
```qml
property int volume: 50

Slider {
	value: volume
	onRightPressed: {
		volume = Math.min(100, volume + 5);
		musicPlayer.setVolume(volume);
	}
}
```
```cpp
class MusicPlayer : public QObject {
	Q_PROPERTY(int volume READ volume NOTIFY ...)
public:
	int volume() const { return m_volume; }
	
	Q_INVOKABLE void increaseVolume() {
		m_volume = qMin(100, volume + 5);
		m_backend->setVolume(m_volume);
	}
private:
	int m_volume = EAR_BLEEDING_LOUD;
}
```
```qml
Slider {
	value: musicPlayer.volume
	onRightPressed: musicPlayer.increaseVolume();
}
```
## Performance
### Tip #14 Make components truly reusable
```qml
// PlayListView.qml
CheckBox { id: showAlbumArt }

ListView {
	model. musicPlayer.playList
	delegate: SongInfo {}
}
```
```qml
// SongInfo.qml
Item {
	width: parent.width
	opacity: ListView.isCurrentItem ? 1 : 0.5

	Image {
		visible: showAlbumArt.checked
		source: model.album + ".png"
	}

	Text { text: model.title }
}
```
```qml
// PlayListView.qml
CheckBox { id: showAlbumArtCheck }

ListView {
	model. musicPlayer.playList
	delegate: SongInfo {
		width: parent.width
		album: model.album
		title: model.title
		highlight: ListView.isCurrentItem
		showAlbumArt: showAlbumArtCheck.checked
	}
}
```
```qml
// SongInfo.qml
Item {
	property bool highlight: false
	property alias showAlbumArt: album.visible
	property string album
	property string title
	opacity: highlight ? 1 : 0.5

	Image { source: model.album + ".png" }
	Text { text: title }
}
```
- Avoid referencing elements from the outer contexts
	- `parent` of the root
	- IDs from other files (especially evil!)
	- Attached properties
	- Non-global context properties
- Avoid settings anchors, positions and explicit sizes on the root
### Tip #15 Load complex elements on demand
- Easier way to reduce load time and complexity
- Unloading also stops binding updates
- `Loader::active` property useful
### Tip #16 Don't block the main thread
- Make `Loader` async
- Make `Image` async
- Don't block in C++ code invoked from QML
	- property access
	- `Q_INVOKABLE` methods and slots
	- model access
	- rendering
	- ...
### Tip #17 Render only what is needed
```qml
// main.qml
Rectangle {
	anchors.fill: parent
	color: Style.backgroundColor

	Toolbar { .. }
}
```
```qml
// Toolbar.qml
Rectangle {
	color: Style.backgroundColor

	Toolbutton { .. }
	Toolbutton { .. }
}
```
```qml
// ToolButton.qml
Rectangle {
	color: Style.backgroundColor

	image: { source: "toolbutton.png" }
}
```
```qml
// main.qml
Item {
	anchors.fill: parent
	Toolbar { .. }
}
```
```qml
// Toolbar.qml
Rectangle {
	Toolbutton { .. }
	Toolbutton { .. }
}
```
```qml
// ToolButton.qml
Rectangle {
	image: { source: "toolbutton.png" }
}
```
```cpp
// main.cpp
view.setColor(style.backgroundColor);
```
### Tip #18 Use the QtQuick Compiler
## Bonus Slides