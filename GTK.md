
# GTK 4.0 C++ Development Guide

## Table of Contents
1. [Introduction to GTK 4.0](#introduction-to-gtk-40)
2. [Setting Up Your Development Environment](#setting-up-your-development-environment)
3. [Creating Your First GTK 4.0 Application](#creating-your-first-gtk-40-application)
4. [Basic Concepts and Widgets](#basic-concepts-and-widgets)
5. [Layout and Containers](#layout-and-containers)
6. [Signals and Event Handling](#signals-and-event-handling)
7. [Building Advanced User Interfaces](#building-advanced-user-interfaces)
8. [Working with GObject and GLib](#working-with-gobject-and-glib)
9. [Styling with CSS](#styling-with-css)
10. [Handling Files and Resources](#handling-files-and-resources)
11. [Multithreading and Asynchronous Operations](#multithreading-and-asynchronous-operations)
12. [Best Practices and Optimization](#best-practices-and-optimization)
13. [Common Examples and Patterns](#common-examples-and-patterns)
14. [Troubleshooting](#troubleshooting)
15. [References and Further Reading](#references-and-further-reading)

## Introduction to GTK 4.0

GTK (GIMP Toolkit) is a free and open-source cross-platform widget toolkit for creating graphical user interfaces. GTK 4.0 represents a major evolution from the GTK 3.x series, with significant improvements in architecture, performance, and features.

### Key Changes in GTK 4.0
- New rendering architecture using GPU acceleration
- Improved input handling with gesture recognition
- Redesigned layout system
- Consolidated theme API
- Removal of deprecated APIs from GTK 3.x
- Better integration with modern graphics APIs

### Why Use C++ with GTK?
While GTK itself is written in C, it works well with C++ through various bindings. The most common approaches for using GTK with C++ are:

1. **Direct use of the C API**: Using the C functions directly from C++
2. **gtkmm**: The official C++ binding for GTK
3. **Custom C++ wrappers**: Creating your own wrapper classes

This guide will primarily focus on using gtkmm 4.0, which provides a complete C++ interface to GTK.

## Setting Up Your Development Environment

### Prerequisites
Before you start developing GTK 4.0 applications with C++, ensure you have the following installed:

#### For Ubuntu/Debian
```bash
sudo apt update
sudo apt install build-essential git meson ninja-build libgtk-4-dev libgtkmm-4.0-dev pkg-config
```

#### For Fedora
```bash
sudo dnf install gcc-c++ git meson ninja-build gtk4-devel gtkmm4-devel pkg-config
```

#### For Arch Linux
```bash
sudo pacman -S base-devel git meson ninja gtk4 gtkmm-4.0 pkgconf
```

#### For macOS (using Homebrew)
```bash
brew install meson ninja pkg-config gtk4 gtkmm4
```

#### For Windows
For Windows, the simplest approach is to use MSYS2:

1. Download and install MSYS2 from https://www.msys2.org/
2. Open MSYS2 MINGW64 shell
3. Run:
```bash
pacman -Syu
pacman -S mingw-w64-x86_64-gcc mingw-w64-x86_64-meson mingw-w64-x86_64-ninja mingw-w64-x86_64-pkg-config mingw-w64-x86_64-gtk4 mingw-w64-x86_64-gtkmm-4.0 git
```

### Setting Up a Project with Meson

Meson is the recommended build system for GTK applications. Here's how to set up a basic project:

1. Create a new directory for your project:
```bash
mkdir my-gtk-app
cd my-gtk-app
```

1. Create a `meson.build` file:
```meson
project('my-gtk-app', 'cpp',
  version : '0.1',
  default_options : ['warning_level=3', 'cpp_std=c++17'])

# Dependencies
gtkmm_dep = dependency('gtkmm-4.0')

# Source files
sources = [
  'src/main.cpp',
  'src/window.cpp',
]

# Headers directory
inc_dir = include_directories('include')

# Executable
executable('my-gtk-app',
  sources,
  include_directories : inc_dir,
  dependencies : gtkmm_dep,
  install : true)
```

1. Create the directory structure:
```bash
mkdir -p src include
```

1. Create a basic header file in `include/window.h`:
```cpp
#pragma once

#include <gtkmm.h>

class MainWindow : public Gtk::Window {
public:
  MainWindow();
  virtual ~MainWindow();

protected:
  // Widgets
  Gtk::Box m_box;
  Gtk::Button m_button;

  // Signal handlers
  void on_button_clicked();
};
```

1. Create the implementation in `src/window.cpp`:
```cpp
#include "window.h"

MainWindow::MainWindow() :
  m_box(Gtk::Orientation::VERTICAL),
  m_button("Click Me")
{
  set_title("GTK 4.0 C++ Example");
  set_default_size(400, 300);

  // Setup button
  m_button.signal_clicked().connect(sigc::mem_fun(*this, &MainWindow::on_button_clicked));

  // Add widgets to the box
  m_box.append(m_button);
  m_box.set_margin(10);
  
  // Set the main container
  set_child(m_box);
}

MainWindow::~MainWindow() {
}

void MainWindow::on_button_clicked() {
  std::cout << "Button clicked!" << std::endl;
}
```

1. Create the main file in `src/main.cpp`:
```cpp
#include "window.h"
#include <gtkmm/application.h>

int main(int argc, char* argv[]) {
  auto app = Gtk::Application::create("org.example.myapp");
  
  return app->make_window_and_run<MainWindow>(argc, argv);
}
```

1. Build the project:
```bash
meson setup builddir
cd builddir
ninja
```

1. Run your application:
```bash
./my-gtk-app
```

## Creating Your First GTK 4.0 Application

Let's look in more detail at the "Hello, World!" example we just created.

### Understanding the Basic Structure

#### The Application Class
The `Gtk::Application` class handles application lifecycle and integration with the desktop environment:

```cpp
auto app = Gtk::Application::create("org.example.myapp");
```

The string parameter represents the application ID, which should be in reverse domain name notation.

#### The Window Class
Our `MainWindow` class inherits from `Gtk::Window`, which is the base for all top-level windows:

```cpp
class MainWindow : public Gtk::Window {
  // ...
};
```

#### The Main Function
The main function creates the application and starts the main loop:

```cpp
int main(int argc, char* argv[]) {
  auto app = Gtk::Application::create("org.example.myapp");
  return app->make_window_and_run<MainWindow>(argc, argv);
}
```

### Extending the Example

Let's add a few more widgets to make our example more interesting:

```cpp
// In window.h
class MainWindow : public Gtk::Window {
public:
  MainWindow();
  virtual ~MainWindow();

protected:
  // Widgets
  Gtk::Box m_box;
  Gtk::Label m_label;
  Gtk::Entry m_entry;
  Gtk::Button m_button;

  // Signal handlers
  void on_button_clicked();
};

// In window.cpp
MainWindow::MainWindow() :
  m_box(Gtk::Orientation::VERTICAL, 10),
  m_label("Enter your name:"),
  m_button("Greet")
{
  set_title("GTK 4.0 C++ Example");
  set_default_size(400, 300);

  // Setup button
  m_button.signal_clicked().connect(sigc::mem_fun(*this, &MainWindow::on_button_clicked));

  // Add widgets to the box
  m_box.append(m_label);
  m_box.append(m_entry);
  m_box.append(m_button);
  m_box.set_margin(10);
  
  // Set the main container
  set_child(m_box);
}

void MainWindow::on_button_clicked() {
  auto name = m_entry.get_text();
  if (name.empty()) {
    name = "stranger";
  }
  
  auto dialog = Gtk::AlertDialog::create("Hello, " + name + "!");
  dialog->show(*this);
}
```

## Basic Concepts and Widgets

### Core GTK Concepts

#### Widgets
Widgets are the building blocks of GTK applications. They can be visible UI elements (like buttons and labels) or containers that organize other widgets (like boxes and grids).

#### Properties
Widgets have properties that control their appearance and behavior. You can set these properties using the `set_property` method or specific setter methods.

```cpp
// Using the specific method
button.set_label("Click Me");

// Using the generic property system
button.set_property("label", "Click Me");
```

#### Signals
Signals are emitted when something happens to a widget (like a button click). You can connect handlers to these signals to respond to events.

```cpp
button.signal_clicked().connect(sigc::mem_fun(*this, &MainWindow::on_button_clicked));
```

### Essential Widgets

#### Windows
`Gtk::Window` is the base class for top-level windows. In GTK 4, each window can have exactly one child widget.

```cpp
// Set a widget as the window's child
window.set_child(box);

// Remove the child
window.set_child(nullptr);
```

#### Boxes
`Gtk::Box` arranges child widgets into a single row or column.

```cpp
// Create a vertical box with 10 pixels spacing between children
Gtk::Box box(Gtk::Orientation::VERTICAL, 10);

// Add widgets
box.append(widget1);
box.append(widget2);

// Control how widgets expand
widget1.set_hexpand(true);  // Expand horizontally
widget2.set_vexpand(true);  // Expand vertically
```

#### Buttons
`Gtk::Button` is a basic button widget.

```cpp
// Create a button with a label
Gtk::Button button("Click Me");

// Connect to the clicked signal
button.signal_clicked().connect([&]() {
  std::cout << "Button clicked!" << std::endl;
});
```

#### Labels
`Gtk::Label` displays text.

```cpp
// Create a label
Gtk::Label label("Hello, World!");

// Set markup (limited HTML-like formatting)
label.set_markup("<b>Bold</b> and <i>italic</i> text");

// Control alignment
label.set_halign(Gtk::Align::START);
```

#### Entry
`Gtk::Entry` is a single-line text input field.

```cpp
// Create an entry
Gtk::Entry entry;

// Set initial text
entry.set_text("Initial text");

// Get the text
std::string text = entry.get_text();

// Connect to the changed signal
entry.signal_changed().connect([&]() {
  std::cout << "Text changed to: " << entry.get_text() << std::endl;
});
```

#### CheckButton
`Gtk::CheckButton` is a toggle button with a check mark.

```cpp
// Create a check button
Gtk::CheckButton check("Enable feature");

// Set initial state
check.set_active(true);

// Get the state
bool enabled = check.get_active();

// Connect to the toggled signal
check.signal_toggled().connect([&]() {
  std::cout << "Toggled: " << (check.get_active() ? "ON" : "OFF") << std::endl;
});
```

#### ComboBoxText
`Gtk::ComboBoxText` provides a dropdown list of text options.

```cpp
// Create a combo box
Gtk::ComboBoxText combo;

// Add options
combo.append("option1", "Option 1");
combo.append("option2", "Option 2");
combo.append("option3", "Option 3");

// Set active option
combo.set_active_id("option2");

// Get the active option
std::string active_id = combo.get_active_id();

// Connect to the changed signal
combo.signal_changed().connect([&]() {
  std::cout << "Selected: " << combo.get_active_id() << std::endl;
});
```

#### Scale (Slider)
`Gtk::Scale` provides a slider control.

```cpp
// Create a horizontal scale
Gtk::Scale scale(Gtk::Orientation::HORIZONTAL);

// Set range and step size
scale.set_range(0, 100);
scale.set_increments(1, 10);  // Small step, page step

// Set initial value
scale.set_value(50);

// Get the value
double value = scale.get_value();

// Connect to the value-changed signal
scale.signal_value_changed().connect([&]() {
  std::cout << "Value: " << scale.get_value() << std::endl;
});
```

## Layout and Containers

GTK 4.0 introduces a new layout system that replaces the older container hierarchy with a more consistent approach.

### Box Layout

The `Gtk::Box` widget arranges children in a single row or column:

```cpp
// Create a horizontal box with 5 pixels spacing
Gtk::Box hbox(Gtk::Orientation::HORIZONTAL, 5);

// Create a vertical box with 10 pixels spacing
Gtk::Box vbox(Gtk::Orientation::VERTICAL, 10);

// Add widgets to the box
box.append(widget1);    // Add to the end
box.prepend(widget2);   // Add to the beginning
box.insert_child_after(widget3, widget1);  // Add after a reference widget

// Control spacing and alignment
box.set_homogeneous(true);  // All children get the same space
widget.set_hexpand(true);   // Widget expands horizontally
widget.set_vexpand(true);   // Widget expands vertically
widget.set_halign(Gtk::Align::CENTER);  // Horizontal alignment
widget.set_valign(Gtk::Align::CENTER);  // Vertical alignment
```

### Grid Layout

The `Gtk::Grid` widget arranges children in a table-like grid:

```cpp
// Create a grid
Gtk::Grid grid;

// Attach widgets to the grid: widget, left, top, width, height
grid.attach(widget1, 0, 0, 1, 1);
grid.attach(widget2, 1, 0, 2, 1);
grid.attach(widget3, 0, 1, 3, 2);

// Set spacing between rows and columns
grid.set_row_spacing(10);
grid.set_column_spacing(10);

// Set row and column homogeneity
grid.set_row_homogeneous(true);
grid.set_column_homogeneous(true);
```

### CenterBox Layout

The `Gtk::CenterBox` widget arranges up to three children: start, center, and end:

```cpp
// Create a center box
Gtk::CenterBox centerbox;

// Set the children
centerbox.set_start_widget(leftWidget);
centerbox.set_center_widget(centerWidget);
centerbox.set_end_widget(rightWidget);

// Control spacing
centerbox.set_baseline_position(Gtk::BaselinePosition::CENTER);
```

### Paned Layout

The `Gtk::Paned` widget contains two panes separated by a draggable handle:

```cpp
// Create a horizontal paned container
Gtk::Paned paned(Gtk::Orientation::HORIZONTAL);

// Set the children
paned.set_start_child(leftWidget);
paned.set_end_child(rightWidget);

// Set the position of the divider
paned.set_position(200);
```

### ScrolledWindow

The `Gtk::ScrolledWindow` adds scrollbars to its child when needed:

```cpp
// Create a scrolled window
Gtk::ScrolledWindow scrolled;

// Set the child
scrolled.set_child(widget);

// Control scrollbar visibility
scrolled.set_policy(
  Gtk::PolicyType::AUTOMATIC,  // Horizontal scrollbar
  Gtk::PolicyType::ALWAYS      // Vertical scrollbar
);
```

### Frame

The `Gtk::Frame` draws a border around its child with an optional label:

```cpp
// Create a frame with a label
Gtk::Frame frame("Section Title");

// Set the child
frame.set_child(widget);

// Control label alignment
frame.set_label_align(Gtk::Align::CENTER);
```

### Notebook (Tabs)

The `Gtk::Notebook` provides a tabbed interface:

```cpp
// Create a notebook
Gtk::Notebook notebook;

// Add pages with tabs
notebook.append_page(page1, *Gtk::make_managed<Gtk::Label>("Tab 1"));
notebook.append_page(page2, *Gtk::make_managed<Gtk::Label>("Tab 2"));

// Connect to tab-changed signal
notebook.signal_switch_page().connect([&](Gtk::Widget*, guint page_num) {
  std::cout << "Switched to page: " << page_num << std::endl;
});
```

### Stack and StackSwitcher

The `Gtk::Stack` and `Gtk::StackSwitcher` provide a modern alternative to the notebook:

```cpp
// Create a stack
Gtk::Stack stack;

// Add pages to the stack
stack.add(page1, "page1", "Page 1");
stack.add(page2, "page2", "Page 2");

// Create a stack switcher
Gtk::StackSwitcher switcher;
switcher.set_stack(stack);

// Now arrange them in your UI
box.append(switcher);
box.append(stack);

// The stack switcher will automatically display buttons for switching pages
```

## Signals and Event Handling

GTK uses a signal system for event handling. Signals are emitted when something happens to a widget, and you can connect handlers to these signals to respond to events.

### Basic Signal Connection

```cpp
// Connect a member function to a signal
button.signal_clicked().connect(
  sigc::mem_fun(*this, &MyClass::on_button_clicked));

// Connect a lambda function to a signal
button.signal_clicked().connect([&]() {
  std::cout << "Button clicked!" << std::endl;
});
```

### Signal Handler with Parameters

Some signals pass parameters to the handler:

```cpp
// Connect to a signal with parameters
entry.signal_key_press_event().connect(
  [&](guint keyval, guint keycode, Gdk::ModifierType state) -> bool {
    std::cout << "Key pressed: " << keyval << std::endl;
    return false;  // Return true to stop the signal
  });
```

### Common Signals

#### Window Signals
- `signal_close_request()`: Emitted when the window is closed
- `signal_show()`: Emitted when the window is shown
- `signal_hide()`: Emitted when the window is hidden

#### Widget Signals
- `signal_realize()`: Emitted when the widget is realized
- `signal_unrealize()`: Emitted when the widget is unrealized
- `signal_map()`: Emitted when the widget is mapped
- `signal_unmap()`: Emitted when the widget is unmapped

#### Button Signals
- `signal_clicked()`: Emitted when the button is clicked

#### Entry Signals
- `signal_changed()`: Emitted when the entry's text changes
- `signal_activate()`: Emitted when Enter is pressed in the entry

#### ComboBox Signals
- `signal_changed()`: Emitted when the selection changes

#### Scale Signals
- `signal_value_changed()`: Emitted when the value changes

#### Gesture Signals
GTK 4.0 introduces a new gesture system for handling input:

```cpp
// Create a click gesture
auto click_gesture = Gtk::GestureClick::create();
widget.add_controller(click_gesture);

// Connect to the pressed signal
click_gesture->signal_pressed().connect(
  [&](int n_press, double x, double y) {
    std::cout << "Pressed at (" << x << ", " << y << ")" << std::endl;
  });
```

### Advanced Signal Handling

#### Using sigc::bind

You can bind additional parameters to a signal handler:

```cpp
// Create a handler that takes an ID
void MyClass::on_button_clicked(int id) {
  std::cout << "Button " << id << " clicked!" << std::endl;
}

// Connect with a bound parameter
button1.signal_clicked().connect(
  sigc::bind(sigc::mem_fun(*this, &MyClass::on_button_clicked), 1));
button2.signal_clicked().connect(
  sigc::bind(sigc::mem_fun(*this, &MyClass::on_button_clicked), 2));
```

#### Using sigc::track_obj

You can track an object's lifetime to automatically disconnect signals:

```cpp
// Connect with object tracking
button.signal_clicked().connect(
  sigc::track_obj([&]() {
    std::cout << "Button clicked!" << std::endl;
  }, *this));  // Signal will be disconnected when *this is destroyed
```

#### Signal Blocking

You can temporarily block signals:

```cpp
// Block a signal
auto connection = button.signal_clicked().connect([&]() { /* ... */ });
connection.block();  // Block the signal

// Do something without triggering the signal
button.clicked();

// Unblock the signal
connection.unblock();
```

## Building Advanced User Interfaces

### HeaderBar and Modern UI

GTK 4.0 encourages a modern UI style with a header bar instead of a traditional title bar:

```cpp
// Create a header bar
auto header = Gtk::make_managed<Gtk::HeaderBar>();

// Add buttons to the header bar
auto menu_button = Gtk::make_managed<Gtk::MenuButton>();
menu_button->set_icon_name("open-menu-symbolic");
header->pack_end(*menu_button);

// Set the header bar as the title bar
set_titlebar(*header);
```

### ListBox for Dynamic Lists

`Gtk::ListBox` provides a flexible way to display and interact with lists:

```cpp
// Create a list box
Gtk::ListBox listbox;

// Add rows
for (int i = 1; i <= 10; i++) {
  auto row = Gtk::make_managed<Gtk::ListBoxRow>();
  auto label = Gtk::make_managed<Gtk::Label>("Item " + std::to_string(i));
  row->set_child(*label);
  listbox.append(*row);
}

// Connect to the row-activated signal
listbox.signal_row_activated().connect([&](Gtk::ListBoxRow* row) {
  int index = row->get_index();
  std::cout << "Row " << index << " activated!" << std::endl;
});
```

### Using ListView and ColumnView (New in GTK 4)

GTK 4 introduces new list views based on a factory pattern:

```cpp
// Create a string list as a model
auto model = Gtk::StringList::create({"Item 1", "Item 2", "Item 3"});

// Create a single-selection model
auto selection = Gtk::SingleSelection::create(model);

// Create a list view with the selection model
auto listview = Gtk::make_managed<Gtk::ListView>(selection);

// Create a factory for list items
auto factory = Gtk::SignalListItemFactory::create();
factory->signal_setup().connect([](const Glib::RefPtr<Gtk::ListItem>& list_item) {
  list_item->set_child(*Gtk::make_managed<Gtk::Label>());
});
factory->signal_bind().connect([](const Glib::RefPtr<Gtk::ListItem>& list_item) {
  auto label = dynamic_cast<Gtk::Label*>(list_item->get_child());
  auto item = std::dynamic_pointer_cast<Gtk::StringObject>(list_item->get_item());
  label->set_text(item->get_string());
});

// Set the factory for the list view
listview->set_factory(factory);
```

### Dialogs

GTK 4.0 simplifies dialogs with new classes like `Gtk::AlertDialog`:

```cpp
// Create an alert dialog
auto dialog = Gtk::AlertDialog::create("This is an important message!");
dialog->set_detail("More details about the alert.");

// Show the dialog
dialog->show(*this);  // Pass the parent window
```

For more complex dialogs, you can create a custom dialog class:

```cpp
class MyDialog : public Gtk::Dialog {
public:
  MyDialog(Gtk::Window& parent, const std::string& title)
    : Gtk::Dialog(title, parent, true)  // true for modal
  {
    // Create the dialog content
    auto content_area = get_content_area();
    
    m_label.set_text("Please enter your name:");
    content_area->append(m_label);
    
    content_area->append(m_entry);
    
    // Add buttons
    add_button("Cancel", Gtk::ResponseType::CANCEL);
    add_button("OK", Gtk::ResponseType::OK);
    
    // Show all widgets
    show();
  }
  
  std::string get_name() const {
    return m_entry.get_text();
  }
  
private:
  Gtk::Label m_label;
  Gtk::Entry m_entry;
};

// Using the dialog
auto dialog = Gtk::make_managed<MyDialog>(*this, "User Information");
int response = dialog->run();

if (response == Gtk::ResponseType::OK) {
  std::string name = dialog->get_name();
  std::cout << "Name: " << name << std::endl;
}
```

### Application Menu and Actions

GTK 4.0 uses a modern approach to menus and actions:

```cpp
// Add actions to the application
auto app = dynamic_cast<Gtk::Application*>(get_application());
app->add_action("quit", [&]() {
  app->quit();
});
app->add_action("about", [&]() {
  auto dialog = Gtk::AboutDialog::create();
  dialog->set_program_name("My GTK App");
  dialog->set_version("1.0");
  dialog->set_authors({"Your Name"});
  dialog->set_license_type(Gtk::License::GPL_3_0);
  dialog->set_transient_for(*this);
  dialog->show();
});

// Create a menu model
auto menu_model = Gio::Menu::create();
menu_model->append("About", "app.about");
menu_model->append("Quit", "app.quit");

// Set up a menu button
auto menu_button = Gtk::make_managed<Gtk::MenuButton>();
menu_button->set_icon_name("open-menu-symbolic");
menu_button->set_menu_model(menu_model);
header->pack_end(*menu_button);
```

### Keyboard Shortcuts

```cpp
// Add keyboard shortcuts
app->set_accels_for_action("app.quit", {"<Ctrl>q"});
app->set_accels_for_action("app.about", {"F1"});
```

## Working with GObject and GLib

GTK is built on the GObject and GLib libraries, which provide fundamental functionality.

### GObject Property System

GObject provides a property system that allows you to get and set properties of objects:

```cpp
// Set a property
button.set_property("label", "Click Me");

// Get a property
Glib::ustring label;
button.get_property("label", label);
```

### Using GLib Utilities

GLib provides many utility functions and classes:

#### Strings and Text

```cpp
// Convert a string to lowercase
Glib::ustring text = "Hello, World!";
Glib::ustring lower = text.lowercase();

// Split a string
std::vector<Glib::ustring> parts = text.split(",");

// Format a string
Glib::ustring formatted = Glib::ustring::format("Value: ", 42);
```

#### File Operations

```cpp
// Check if a file exists
bool exists = Glib::file_test("file.txt", Glib::FileTest::EXISTS);

// Create a directory
bool success = Glib::mkdir_with_parents("path/to/dir", 0755);

// Get the contents of a file
std::string contents;
bool success = Glib::file_get_contents("file.txt", contents);
```

#### Date and Time

```cpp
// Get the current time
auto now = Glib::DateTime::create_now_local();

// Format a date/time
std::string formatted = now->format("%Y-%m-%d %H:%M:%S");

// Calculate the difference between two times
auto then = Glib::DateTime::create_from_iso8601("2023-01-01T00:00:00Z");
auto diff = now->difference(then);  // Difference in microseconds
```

### GLib::MainLoop and Timeout

```cpp
// Add a timeout function that runs every 1000 milliseconds (1 second)
auto connection = Glib::signal_timeout().connect(
  [&]() -> bool {
    std::cout << "Timeout!" << std::endl;
    return true;  // Return true to continue, false to stop
  },
  1000
);

// Later, disconnect the timeout
connection.disconnect();
```

### Custom GObject Types

You can create custom GObject types using the gtkmm macros:

```cpp
class MyObject : public Glib::Object {
public:
  static Glib::RefPtr<MyObject> create() {
    return Glib::make_refptr_for_instance<MyObject>(new MyObject());
  }

  // Property accessor methods
  int get_count() const { return m_count; }
  void set_count(int count) { m_count = count; property_count().emit_changed(); }

  // Property
  Glib::PropertyProxy<int> property_count() {
    return m_property_count.get_proxy();
  }

  // Signal
  using type_signal_changed = sigc::signal<void(int)>;
  type_signal_changed signal_changed() { return m_signal_changed; }

protected:
  MyObject() : m_property_count(*this, "count", 0) {
    property_count().signal_changed().connect(
      [this]() {
        m_signal_changed.emit(m_count);
      });
  }

private:
  int m_count;
  Glib::Property<int> m_property_count;
  type_signal_changed m_signal_changed;
};
```

## Styling with CSS

GTK 4.0 uses CSS for styling widgets.

### Basic CSS Styling

```cpp
// Load CSS from a string
auto provider = Gtk::CssProvider::create();
provider->load_from_data(
  "button { background-color: #3584e4; color: white; }"
  "button:hover { background-color: #729fcf; }"
);

// Apply the CSS to the default screen
auto display = Gdk::Display::get_default();
Gtk::StyleContext::add_provider_for_display(
  display,
  provider,
  GTK_STYLE_PROVIDER_PRIORITY_APPLICATION
);
```

### Widget-Specific Styling

You can also apply CSS to specific widgets:

```cpp
// Create a CSS provider
auto provider = Gtk::CssProvider::create();
provider->load_from_data(
  "button.important { background-color: red; color: white; }"
);

// Add a CSS class to a widget
button.add_css_class("important");

// Apply the provider to a specific widget
button.get_style_context()->add_provider(
  provider,
  GTK_STYLE_PROVIDER_PRIORITY_APPLICATION
);
```

### Loading CSS from a File

```cpp
// Load CSS from a file
auto provider = Gtk::CssProvider::create();
try {
  provider->load_from_path("style.css");
  
  auto display = Gdk::Display::get_default();
  Gtk::StyleContext::add_provider_for_display(
    display,
    provider,
    GTK_STYLE_PROVIDER_PRIORITY_APPLICATION
  );
} catch (const Glib::Error& ex) {
  std::cerr << "Failed to load CSS: " << ex.what() << std::endl;
}
```

### Common CSS Properties

- `background-color`: Background color
- `color`: Text color
- `font-family`: Font family
- `font-size`: Font size
- `font-weight`: Font weight (normal, bold)
- `border`: Border properties
- `margin`: Margin around the widget
- `padding`: Padding inside the widget
- `min-width`, `min-height`: Minimum dimensions
- `transition`: Animation properties

### Styling Example

Here's a more complete example of CSS styling:

```css
/* style.css */

/* Global styles */
window {
  background-color: #f6f5f4;
}

/* Header bar */
headerbar {
  background-color: #3584e4;
  color: white;
  font-weight: bold;
}

/* Buttons */
button {
  background-color: #ffffff;
  border: 1px solid #d3d7cf;
  border-radius: 4px;
  padding: 6px 12px;
}

button:hover {
  background-color: #f6f5f4;
}

button:active {
  background-color: #d3d7cf;
}

/* Custom classes */
.important {
  background-color: #e01b24;
  color: white;
  font-weight: bold;
}

.success {
  background-color: #33d17a;
  color: white;
}

/* Widget states */
button:disabled {
  opacity: 0.5;
}

entry:focus {
  border-color: #3584e4;
}
```

## Handling Files and Resources

### Basic File Operations

GTK 4.0 uses GIO for file operations:

```cpp
// Open a file for reading
try {
  auto file = Gio::File::create_for_path("file.txt");
  auto stream = file->read();
  
  // Read the file content
  char buffer[1024];
  gsize bytes_read;
  std::string content;
  
  while ((bytes_read = stream->read(buffer, sizeof(buffer))) > 0) {
    content.append(buffer, bytes_read);
  }
  
  std::cout << "File content: " << content << std::endl;
} catch (const Glib::Error& ex) {
  std::cerr << "Error reading file: " << ex.what() << std::endl;
}
```

### Writing to Files

```cpp
// Write to a file
try {
  auto file = Gio::File::create_for_path("output.txt");
  auto stream = file->replace();
  
  std::string content = "Hello, World!";
  stream->write(content.data(), content.size());
  
  // Don't forget to close the stream
  stream->close();
} catch (const Glib::Error& ex) {
  std::cerr << "Error writing to file: " << ex.what() << std::endl;
}
```

### File Chooser Dialog

GTK 4.0 introduces new file chooser APIs:

```cpp
// Create a file chooser dialog
auto dialog = Gtk::FileDialog::create();
dialog->set_title("Open File");

// Show the dialog
dialog->open(*this, [this](const Glib::RefPtr<Gio::AsyncResult>& result) {
  try {
    auto file = dialog->open_finish(result);
    std::cout << "Selected file: " << file->get_path() << std::endl;
    
    // Do something with the file
  } catch (const Glib::Error& ex) {
    if (ex.code() != Gtk::DialogError::DISMISSED) {
      std::cerr << "Error selecting file: " << ex.what() << std::endl;
    }
  }
});
```

### Resource Bundling

GTK applications can bundle resources (like images, CSS, and UI files) using GResource:

1. Create a resource XML file (e.g., `resources.xml`):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<gresources>
  <gresource prefix="/org/example/myapp">
    <file>style.css</file>
    <file>icons/icon.png</file>
    <file>ui/main-window.ui</file>
  </gresource>
</gresources>
```

1. Compile the resources (in your meson.build):
```meson
gnome = import('gnome')
resources = gnome.compile_resources('resources',
  'resources.xml',
  source_dir: '.',
  c_name: 'resources')

# Add the compiled resources to your executable
executable('my-gtk-app',
  sources + resources,
  include_directories: inc_dir,
  dependencies: gtkmm_dep,
  install: true)
```

1. Load resources in your code:
```cpp
// Load CSS from resources
auto provider = Gtk::CssProvider::create();
provider->load_from_resource("/org/example/myapp/style.css");

// Load an image from resources
auto image = Gtk::Image::create_from_resource("/org/example/myapp/icons/icon.png");

// Load a UI file from resources
auto builder = Gtk::Builder::create_from_resource("/org/example/myapp/ui/main-window.ui");
```

## Multithreading and Asynchronous Operations

GTK is not thread-safe, and all UI operations must be performed from the main thread. However, you can use GLib's threading and asynchronous APIs to perform background operations.

### Basic Threading

```cpp
// Perform a long-running operation in a separate thread
auto thread = Glib::Thread::create([&]() {
  // This code runs in a separate thread
  
  // Perform the long-running operation
  // ...
  
  // Update the UI from the main thread
  Glib::signal_idle().connect_once([&]() {
    // This code runs in the main thread
    label.set_text("Operation completed!");
  });
});
```

### Asynchronous File Operations

```cpp
// Read a file asynchronously
auto file = Gio::File::create_for_path("file.txt");
file->read_async([file](const Glib::RefPtr<Gio::AsyncResult>& result) {
  try {
    auto stream = file->read_finish(result);
    
    // Now read the content asynchronously
    read_stream_async(stream, "");
  } catch (const Glib::Error& ex) {
    std::cerr << "Error reading file: " << ex.what() << std::endl;
  }
});

// Helper function to read a stream asynchronously
void read_stream_async(const Glib::RefPtr<Gio::InputStream>& stream, const std::string& content) {
  char buffer[1024];
  stream->read_async(buffer, sizeof(buffer), [stream, content, buffer](const Glib::RefPtr<Gio::AsyncResult>& result) {
    try {
      gssize bytes_read = stream->read_finish(result);
      if (bytes_read > 0) {
        // Append the read data to the content
        std::string new_content = content + std::string(buffer, bytes_read);
        
        // Continue reading
        read_stream_async(stream, new_content);
      } else {
        // End of file, we have all the content now
        std::cout << "File content: " << content << std::endl;
      }
    } catch (const Glib::Error& ex) {
      std::cerr << "Error reading stream: " << ex.what() << std::endl;
    }
  });
}
```

### Gio::Task for Custom Asynchronous Operations

```cpp
// Define a custom asynchronous operation
void process_data_async(const std::vector<int>& data, const Gio::SlotAsyncReady& slot) {
  auto task = Gio::Task::create(Glib::RefPtr<Glib::Object>(), slot);
  
  // Copy the data to avoid reference issues
  std::vector<int>* data_copy = new std::vector<int>(data);
  
  task->set_task_data(data_copy, [](void* data) {
    delete static_cast<std::vector<int>*>(data);
  });
  
  // Run the task in a thread
  task->run_in_thread([](const Glib::RefPtr<Gio::Task>& task,
                        Glib::RefPtr<Glib::Object> source_object,
                        void* task_data) {
    auto data = static_cast<std::vector<int>*>(task_data);
    
    // Perform the long-running operation
    int sum = 0;
    for (int value : *data) {
      sum += value;
      // Simulate a long operation
      std::this_thread::sleep_for(std::chrono::milliseconds(10));
      
      // Check if the task has been cancelled
      if (task->return_error_if_cancelled()) {
        return;
      }
    }
    
    // Return the result
    task->return_value(Glib::Variant<int>::create(sum));
  });
}

// Function to finish the asynchronous operation
int process_data_finish(const Glib::RefPtr<Gio::AsyncResult>& result) {
  auto task = Glib::RefPtr<Gio::Task>::cast_dynamic(result);
  return task->get_result().get<int>();
}

// Using the asynchronous operation
std::vector<int> data = {1, 2, 3, 4, 5};
process_data_async(data, [&](const Glib::RefPtr<Gio::AsyncResult>& result) {
  try {
    int sum = process_data_finish(result);
    std::cout << "Sum: " << sum << std::endl;
  } catch (const Glib::Error& ex) {
    std::cerr << "Error processing data: " << ex.what() << std::endl;
  }
});
```

### Progress Reporting

```cpp
// Create a progress bar
Gtk::ProgressBar progress_bar;

// Start a long-running operation with progress reporting
auto cancellable = Gio::Cancellable::create();
start_operation(cancellable, [&](double progress) {
  // Update the progress bar from the main thread
  Glib::signal_idle().connect_once([&, progress]() {
    progress_bar.set_fraction(progress);
    
    if (progress >= 1.0) {
      progress_bar.set_text("Completed!");
    } else {
      std::ostringstream oss;
      oss << std::fixed << std::setprecision(1) << (progress * 100) << "%";
      progress_bar.set_text(oss.str());
    }
  });
});

// Add a cancel button
Gtk::Button cancel_button("Cancel");
cancel_button.signal_clicked().connect([&]() {
  cancellable->cancel();
});
```

## Best Practices and Optimization

### Memory Management

GTK uses reference counting for memory management:

- `Glib::RefPtr<>` is a smart pointer that manages the reference count
- `Gtk::make_managed<>()` creates a widget that is owned by its parent container
- Use `Glib::make_refptr_for_instance<>()` to create a `RefPtr` for a newly allocated object

```cpp
// Creating a managed widget
auto button = Gtk::make_managed<Gtk::Button>("Click Me");
box.append(*button);  // The box owns the button

// Creating a RefPtr
auto model = Gtk::StringList::create({"Item 1", "Item 2"});  // Factory function returns RefPtr
```

### Performance Tips

1. **Minimize Redraws**: Batch UI updates to minimize redraws
```cpp
// Block the signal to prevent multiple redraws
listbox.freeze_notify();
for (int i = 0; i < 1000; i++) {
  // Add items to the listbox
}
// Now unblock the signal for a single redraw
listbox.thaw_notify();
```

1. **Use `Gtk::ListBox` for Long Lists**: For long lists, use `Gtk::ListBox` with dynamic row creation
```cpp
// Create a list box with a custom filter function
listbox.set_filter_func([&](Gtk::ListBoxRow* row) -> bool {
  // Return true to show the row, false to hide it
  auto label = dynamic_cast<Gtk::Label*>(row->get_child());
  return label->get_text().find("filter") != std::string::npos;
});
```

1. **Avoid Unnecessary Window Resizing**: Set size requests to prevent frequent resizing
```cpp
window.set_default_size(800, 600);
widget.set_size_request(200, 100);
```

1. **Use CSS for Styling**: Use CSS instead of code for styling
```cpp
// Instead of this:
label.override_color(Gdk::RGBA("#ff0000"));

// Do this:
auto provider = Gtk::CssProvider::create();
provider->load_from_data("label { color: #ff0000; }");
label.get_style_context()->add_provider(provider, GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
```

### Code Organization

1. **Separate UI and Logic**: Keep UI and business logic separate
```cpp
// UI class
class MainWindow : public Gtk::Window {
public:
  MainWindow(MyApplication& app);
  
private:
  MyApplication& m_app;
  Gtk::Box m_box;
  // ...
};

// Application logic class
class MyApplication {
public:
  void process_data();
  // ...
};
```

1. **Use Composite Widgets**: Create custom widgets by composing existing ones
```cpp
class UserInfoWidget : public Gtk::Box {
public:
  UserInfoWidget() :
    Gtk::Box(Gtk::Orientation::VERTICAL, 5),
    m_name_label("Name:"),
    m_email_label("Email:")
  {
    append(m_name_label);
    append(m_name_entry);
    append(m_email_label);
    append(m_email_entry);
    set_margin(10);
  }
  
  std::string get_name() const { return m_name_entry.get_text(); }
  std::string get_email() const { return m_email_entry.get_text(); }
  
private:
  Gtk::Label m_name_label;
  Gtk::Entry m_name_entry;
  Gtk::Label m_email_label;
  Gtk::Entry m_email_entry;
};
```

1. **Use Builder for UI Design**: Separate UI design using Gtk::Builder and UI files
```cpp
// Load UI from a file or resource
auto builder = Gtk::Builder::create_from_file("ui/main-window.ui");

// Get widgets from the builder
Gtk::Window* window;
builder->get_widget("main_window", window);

Gtk::Button* button;
builder->get_widget("button", button);

// Connect signals
button->signal_clicked().connect([&]() {
  std::cout << "Button clicked!" << std::endl;
});
```

### Error Handling

1. **Use Try-Catch for Error Handling**: Catch and handle exceptions properly
```cpp
try {
  // Code that might throw an exception
  auto file = Gio::File::create_for_path("file.txt");
  auto stream = file->read();
  // ...
} catch (const Glib::Error& ex) {
  std::cerr << "Error: " << ex.what() << std::endl;
  show_error_dialog("Failed to open file: " + std::string(ex.what()));
} catch (const std::exception& ex) {
  std::cerr << "Exception: " << ex.what() << std::endl;
  show_error_dialog("An error occurred: " + std::string(ex.what()));
}
```

1. **Show User-Friendly Error Messages**:
```cpp
void show_error_dialog(const std::string& message) {
  auto dialog = Gtk::AlertDialog::create(message);
  dialog->show(*this);  // Pass the parent window
}
```

## Common Examples and Patterns

### Simple Text Editor Example

```cpp
#include <gtkmm.h>
#include <iostream>
#include <fstream>

class TextEditor : public Gtk::Window {
public:
  TextEditor() :
    m_box(Gtk::Orientation::VERTICAL),
    m_save_button("Save"),
    m_open_button("Open")
  {
    set_title("Simple Text Editor");
    set_default_size(800, 600);
    
    // Set up the header bar
    auto header = Gtk::make_managed<Gtk::HeaderBar>();
    header->pack_start(m_open_button);
    header->pack_end(m_save_button);
    set_titlebar(*header);
    
    // Set up the text view
    m_text_buffer = Gtk::TextBuffer::create();
    m_text_view.set_buffer(m_text_buffer);
    m_text_view.set_wrap_mode(Gtk::WrapMode::WORD);
    
    // Add the text view to a scrolled window
    m_scrolled.set_child(m_text_view);
    m_scrolled.set_expand(true);
    
    // Add the scrolled window to the box
    m_box.append(m_scrolled);
    
    // Set the box as the main container
    set_child(m_box);
    
    // Connect signals
    m_save_button.signal_clicked().connect(
      sigc::mem_fun(*this, &TextEditor::on_save_clicked));
    m_open_button.signal_clicked().connect(
      sigc::mem_fun(*this, &TextEditor::on_open_clicked));
    
    // Set up keyboard shortcuts
    auto controller = Gtk::EventControllerKey::create();
    controller->signal_key_pressed().connect(
      sigc::mem_fun(*this, &TextEditor::on_key_pressed), false);
    add_controller(controller);
  }
  
protected:
  void on_save_clicked() {
    auto dialog = Gtk::FileDialog::create();
    dialog->set_title("Save File");
    
    dialog->save(*this, [this](const Glib::RefPtr<Gio::AsyncResult>& result) {
      try {
        auto file = dialog->save_finish(result);
        save_to_file(file->get_path());
      } catch (const Glib::Error& ex) {
        if (ex.code() != Gtk::DialogError::DISMISSED) {
          show_error("Error saving file: " + std::string(ex.what()));
        }
      }
    });
  }
  
  void on_open_clicked() {
    auto dialog = Gtk::FileDialog::create();
    dialog->set_title("Open File");
    
    dialog->open(*this, [this](const Glib::RefPtr<Gio::AsyncResult>& result) {
      try {
        auto file = dialog->open_finish(result);
        open_file(file->get_path());
      } catch (const Glib::Error& ex) {
        if (ex.code() != Gtk::DialogError::DISMISSED) {
          show_error("Error opening file: " + std::string(ex.what()));
        }
      }
    });
  }
  
  bool on_key_pressed(guint keyval, guint keycode, Gdk::ModifierType state) {
    if (state & Gdk::ModifierType::CONTROL_MASK) {
      if (keyval == GDK_KEY_s) {
        on_save_clicked();
        return true;
      } else if (keyval == GDK_KEY_o) {
        on_open_clicked();
        return true;
      }
    }
    return false;
  }
  
  void save_to_file(const std::string& filename) {
    try {
      auto start = m_text_buffer->begin();
      auto end = m_text_buffer->end();
      auto text = m_text_buffer->get_text(start, end, false);
      
      std::ofstream file(filename);
      if (!file) {
        throw std::runtime_error("Could not open file for writing");
      }
      
      file << text;
      file.close();
      
      set_title("Simple Text Editor - " + filename);
    } catch (const std::exception& ex) {
      show_error("Error saving file: " + std::string(ex.what()));
    }
  }
  
  void open_file(const std::string& filename) {
    try {
      std::ifstream file(filename);
      if (!file) {
        throw std::runtime_error("Could not open file for reading");
      }
      
      std::stringstream buffer;
      buffer << file.rdbuf();
      
      m_text_buffer->set_text(buffer.str());
      set_title("Simple Text Editor - " + filename);
    } catch (const std::exception& ex) {
      show_error("Error opening file: " + std::string(ex.what()));
    }
  }
  
  void show_error(const std::string& message) {
    auto dialog = Gtk::AlertDialog::create(message);
    dialog->show(*this);
  }
  
private:
  Gtk::Box m_box;
  Gtk::ScrolledWindow m_scrolled;
  Gtk::TextView m_text_view;
  Glib::RefPtr<Gtk::TextBuffer> m_text_buffer;
  Gtk::Button m_save_button;
  Gtk::Button m_open_button;
};

int main(int argc, char* argv[]) {
  auto app = Gtk::Application::create("org.example.texteditor");
  return app->make_window_and_run<TextEditor>(argc, argv);
}
```

### Data Visualization Example

```cpp
#include <gtkmm.h>
#include <vector>
#include <cmath>

class DrawingArea : public Gtk::DrawingArea {
public:
  DrawingArea() {
    // Set up the drawing area
    set_draw_func(sigc::mem_fun(*this, &DrawingArea::on_draw));
    set_content_width(800);
    set_content_height(400);
    
    // Generate some data
    for (int i = 0; i < 100; i++) {
      double x = i / 10.0;
      double y = std::sin(x);
      m_data.push_back({x, y});
    }
  }
  
protected:
  void on_draw(const Cairo::RefPtr<Cairo::Context>& cr, int width, int height) {
    // Clear the background
    cr->set_source_rgb(1.0, 1.0, 1.0);
    cr->paint();
    
    // Set up the coordinate system
    const double padding = 40;
    const double graph_width = width - 2 * padding;
    const double graph_height = height - 2 * padding;
    
    // Find the data range
    double x_min = m_data[0].first;
    double x_max = m_data[0].first;
    double y_min = m_data[0].second;
    double y_max = m_data[0].second;
    
    for (const auto& point : m_data) {
      x_min = std::min(x_min, point.first);
      x_max = std::max(x_max, point.first);
      y_min = std::min(y_min, point.second);
      y_max = std::max(y_max, point.second);
    }
    
    // Add some margin to the y range
    double y_margin = (y_max - y_min) * 0.1;
    y_min -= y_margin;
    y_max += y_margin;
    
    // Draw the axes
    cr->set_source_rgb(0.0, 0.0, 0.0);
    cr->set_line_width(1);
    
    // X axis
    cr->move_to(padding, height - padding);
    cr->line_to(width - padding, height - padding);
    cr->stroke();
    
    // Y axis
    cr->move_to(padding, padding);
    cr->line_to(padding, height - padding);
    cr->stroke();
    
    // Draw the data points
    cr->set_source_rgb(0.0, 0.5, 1.0);
    cr->set_line_width(2);
    
    bool first = true;
    for (const auto& point : m_data) {
      // Map the data point to screen coordinates
      double x = padding + graph_width * (point.first - x_min) / (x_max - x_min);
      double y = height - padding - graph_height * (point.second - y_min) / (y_max - y_min);
      
      if (first) {
        cr->move_to(x, y);
        first = false;
      } else {
        cr->line_to(x, y);
      }
    }
    
    cr->stroke();
    
    // Draw labels
    cr->set_source_rgb(0.0, 0.0, 0.0);
    
    // X axis label
    cr->move_to(width / 2, height - padding / 2);
    cr->set_font_size(12);
    cr->show_text("X");
    
    // Y axis label
    cr->save();
    cr->move_to(padding / 2, height / 2);
    cr->rotate(-M_PI / 2);
    cr->show_text("sin(x)");
    cr->restore();
    
    // Title
    cr->move_to(width / 2 - 50, padding / 2);
    cr->set_font_size(16);
    cr->show_text("Sine Wave");
  }
  
private:
  std::vector<std::pair<double, double>> m_data;
};

class VisualizationWindow : public Gtk::Window {
public:
  VisualizationWindow() {
    set_title("Data Visualization");
    set_default_size(800, 400);
    
    // Set up the drawing area
    m_box.append(m_drawing_area);
    
    // Set the box as the main container
    set_child(m_box);
  }
  
private:
  Gtk::Box m_box{Gtk::Orientation::VERTICAL};
  DrawingArea m_drawing_area;
};

int main(int argc, char* argv[]) {
  auto app = Gtk::Application::create("org.example.visualization");
  return app->make_window_and_run<VisualizationWindow>(argc, argv);
}
```

### Database Application Example

```cpp
#include <gtkmm.h>
#include <sqlite3.h>
#include <iostream>
#include <vector>
#include <memory>

// Simple wrapper for SQLite
class Database {
public:
  Database(const std::string& filename) : m_db(nullptr) {
    int rc = sqlite3_open(filename.c_str(), &m_db);
    if (rc != SQLITE_OK) {
      std::string error_msg = sqlite3_errmsg(m_db);
      sqlite3_close(m_db);
      m_db = nullptr;
      throw std::runtime_error("Cannot open database: " + error_msg);
    }
    
    // Create the table if it doesn't exist
    const char* sql = "CREATE TABLE IF NOT EXISTS contacts ("
                     "id INTEGER PRIMARY KEY AUTOINCREMENT,"
                     "name TEXT NOT NULL,"
                     "email TEXT,"
                     "phone TEXT);";
    
    char* error_msg = nullptr;
    rc = sqlite3_exec(m_db, sql, nullptr, nullptr, &error_msg);
    if (rc != SQLITE_OK) {
      std::string error = error_msg;
      sqlite3_free(error_msg);
      throw std::runtime_error("SQL error: " + error);
    }
  }
  
  ~Database() {
    if (m_db) {
      sqlite3_close(m_db);
    }
  }
  
  struct Contact {
    int id;
    std::string name;
    std::string email;
    std::string phone;
  };
  
  std::vector<Contact> get_all_contacts() {
    std::vector<Contact> contacts;
    
    const char* sql = "SELECT id, name, email, phone FROM contacts ORDER BY name;";
    sqlite3_stmt* stmt = nullptr;
    
    int rc = sqlite3_prepare_v2(m_db, sql, -1, &stmt, nullptr);
    if (rc != SQLITE_OK) {
      throw std::runtime_error("Failed to prepare statement: " + std::string(sqlite3_errmsg(m_db)));
    }
    
    while ((rc = sqlite3_step(stmt)) == SQLITE_ROW) {
      Contact contact;
      contact.id = sqlite3_column_int(stmt, 0);
      const char* name = reinterpret_cast<const char*>(sqlite3_column_text(stmt, 1));
      const char* email = reinterpret_cast<const char*>(sqlite3_column_text(stmt, 2));
      const char* phone = reinterpret_cast<const char*>(sqlite3_column_text(stmt, 3));
      
      contact.name = name ? name : "";
      contact.email = email ? email : "";
      contact.phone = phone ? phone : "";
      
      contacts.push_back(contact);
    }
    
    sqlite3_finalize(stmt);
    
    if (rc != SQLITE_DONE) {
      throw std::runtime_error("Error reading contacts: " + std::string(sqlite3_errmsg(m_db)));
    }
    
    return contacts;
  }
  
  void add_contact(const std::string& name, const std::string& email, const std::string& phone) {
    const char* sql = "INSERT INTO contacts (name, email, phone) VALUES (?, ?, ?);";
    sqlite3_stmt* stmt = nullptr;
    
    int rc = sqlite3_prepare_v2(m_db, sql, -1, &stmt, nullptr);
    if (rc != SQLITE_OK) {
      throw std::runtime_error("Failed to prepare statement: " + std::string(sqlite3_errmsg(m_db)));
    }
    
    sqlite3_bind_text(stmt, 1, name.c_str(), -1, SQLITE_STATIC);
    sqlite3_bind_text(stmt, 2, email.c_str(), -1, SQLITE_STATIC);
    sqlite3_bind_text(stmt, 3, phone.c_str(), -1, SQLITE_STATIC);
    
    rc = sqlite3_step(stmt);
    sqlite3_finalize(stmt);
    
    if (rc != SQLITE_DONE) {
      throw std::runtime_error("Failed to add contact: " + std::string(sqlite3_errmsg(m_db)));
    }
  }
  
  void update_contact(int id, const std::string& name, const std::string& email, const std::string& phone) {
    const char* sql = "UPDATE contacts SET name = ?, email = ?, phone = ? WHERE id = ?;";
    sqlite3_stmt* stmt = nullptr;
    
    int rc = sqlite3_prepare_v2(m_db, sql, -1, &stmt, nullptr);
    if (rc != SQLITE_OK) {
      throw std::runtime_error("Failed to prepare statement: " + std::string(sqlite3_errmsg(m_db)));
    }
    
    sqlite3_bind_text(stmt, 1, name.c_str(), -1, SQLITE_STATIC);
    sqlite3_bind_text(stmt, 2, email.c_str(), -1, SQLITE_STATIC);
    sqlite3_bind_text(stmt, 3, phone.c_str(), -1, SQLITE_STATIC);
    sqlite3_bind_int(stmt, 4, id);
    
    rc = sqlite3_step(stmt);
    sqlite3_finalize(stmt);
    
    if (rc != SQLITE_DONE) {
      throw std::runtime_error("Failed to update contact: " + std::string(sqlite3_errmsg(m_db)));
    }
  }
  
  void delete_contact(int id) {
    const char* sql = "DELETE FROM contacts WHERE id = ?;";
    sqlite3_stmt* stmt = nullptr;
    
    int rc = sqlite3_prepare_v2(m_db, sql, -1, &stmt, nullptr);
    if (rc != SQLITE_OK) {
      throw std::runtime_error("Failed to prepare statement: " + std::string(sqlite3_errmsg(m_db)));
    }
    
    sqlite3_bind_int(stmt, 1, id);
    
    rc = sqlite3_step(stmt);
    sqlite3_finalize(stmt);
    
    if (rc != SQLITE_DONE) {
      throw std::runtime_error("Failed to delete contact: " + std::string(sqlite3_errmsg(m_db)));
    }
  }
  
private:
  sqlite3* m_db;
};

// Contact list model class for the list view
class ContactListModel : public Gtk::ListStore {
public:
  class ContactColumns : public Gtk::TreeModel::ColumnRecord {
  public:
    ContactColumns() {
      add(id);
      add(name);
      add(email);
      add(phone);
    }
    
    Gtk::TreeModelColumn<int> id;
    Gtk::TreeModelColumn<Glib::ustring> name;
    Gtk::TreeModelColumn<Glib::ustring> email;
    Gtk::TreeModelColumn<Glib::ustring> phone;
  };
  
  static Glib::RefPtr<ContactListModel> create(Database& db) {
    return Glib::make_refptr_for_instance<ContactListModel>(new ContactListModel(db));
  }
  
  const ContactColumns& columns() const { return m_columns; }
  
  void refresh() {
    clear();
    
    auto contacts = m_db.get_all_contacts();
    for (const auto& contact : contacts) {
      auto row = append();
      row->set_value(m_columns.id, contact.id);
      row->set_value(m_columns.name, contact.name);
      row->set_value(m_columns.email, contact.email);
      row->set_value(m_columns.phone, contact.phone);
    }
  }
  
  void add_contact(const std::string& name, const std::string& email, const std::string& phone) {
    m_db.add_contact(name, email, phone);
    refresh();
  }
  
  void update_contact(int id, const std::string& name, const std::string& email, const std::string& phone) {
    m_db.update_contact(id, name, email, phone);
    refresh();
  }
  
  void delete_contact(int id) {
    m_db.delete_contact(id);
    refresh();
  }
  
protected:
  ContactListModel(Database& db) : m_db(db) {
    set_column_types(m_columns);
    refresh();
  }
  
private:
  ContactColumns m_columns;
  Database& m_db;
};

// Contact edit dialog
class ContactDialog : public Gtk::Dialog {
public:
  ContactDialog(Gtk::Window& parent, const std::string& title,
                const std::string& name = "", const std::string& email = "", const std::string& phone = "")
    : Gtk::Dialog(title, parent, true)  // true for modal
  {
    // Create the dialog content
    auto content_area = get_content_area();
    content_area->set_margin(10);
    content_area->set_spacing(10);
    
    // Name
    auto name_box = Gtk::make_managed<Gtk::Box>(Gtk::Orientation::HORIZONTAL, 5);
    auto name_label = Gtk::make_managed<Gtk::Label>("Name:");
    name_label->set_width_chars(10);
    name_box->append(*name_label);
    
    m_name_entry.set_hexpand(true);
    m_name_entry.set_text(name);
    name_box->append(m_name_entry);
    
    content_area->append(*name_box);
    
    // Email
    auto email_box = Gtk::make_managed<Gtk::Box>(Gtk::Orientation::HORIZONTAL, 5);
    auto email_label = Gtk::make_managed<Gtk::Label>("Email:");
    email_label->set_width_chars(10);
    email_box->append(*email_label);
    
    m_email_entry.set_hexpand(true);
    m_email_entry.set_text(email);
    email_box->append(m_email_entry);
    
    content_area->append(*email_box);
    
    // Phone
    auto phone_box = Gtk::make_managed<Gtk::Box>(Gtk::Orientation::HORIZONTAL, 5);
    auto phone_label = Gtk::make_managed<Gtk::Label>("Phone:");
    phone_label->set_width_chars(10);
    phone_box->append(*phone_label);
    
    m_phone_entry.set_hexpand(true);
    m_phone_entry.set_text(phone);
    phone_box->append(m_phone_entry);
    
    content_area->append(*phone_box);
    
    // Add buttons
    add_button("Cancel", Gtk::ResponseType::CANCEL);
    add_button("OK", Gtk::ResponseType::OK);
    
    // Show the dialog
    show();
  }
  
  std::string get_name() const { return m_name_entry.get_text(); }
  std::string get_email() const { return m_email_entry.get_text(); }
  std::string get_phone() const { return m_phone_entry.get_text(); }
  
private:
  Gtk::Entry m_name_entry;
  Gtk::Entry m_email_entry;
  Gtk::Entry m_phone_entry;
};

// Main application window
class ContactsWindow : public Gtk::Window {
public:
  ContactsWindow() : m_db("contacts.db"), m_paned(Gtk::Orientation::HORIZONTAL) {
    set_title("Contacts Database");
    set_default_size(800, 600);
    
    // Create the header bar
    auto header = Gtk::make_managed<Gtk::HeaderBar>();
    
    // Add button
    m_add_button.set_icon_name("list-add-symbolic");
    m_add_button.signal_clicked().connect(sigc::mem_fun(*this, &ContactsWindow::on_add_clicked));
    header->pack_start(m_add_button);
    
    // Delete button
    m_delete_button.set_icon_name("edit-delete-symbolic");
    m_delete_button.set_sensitive(false);  // Disable until a selection is made
    m_delete_button.signal_clicked().connect(sigc::mem_fun(*this, &ContactsWindow::on_delete_clicked));
    header->pack_start(m_delete_button);
    
    // Menu button
    auto menu_button = Gtk::make_managed<Gtk::MenuButton>();
    menu_button->set_icon_name("open-menu-symbolic");
    
    auto menu_model = Gio::Menu::create();
    menu_model->append("About", "app.about");
    menu_model->append("Quit", "app.quit");
    
    menu_button->set_menu_model(menu_model);
    header->pack_end(*menu_button);
    
    // Set the header bar as the title bar
    set_titlebar(*header);
    
    // Create the list model
    m_model = ContactListModel::create(m_db);
    
    // Create the tree view
    m_treeview.set_model(m_model);
    m_treeview.append_column("Name", m_model->columns().name);
    m_treeview.append_column("Email", m_model->columns().email);
    m_treeview.append_column("Phone", m_model->columns().phone);
    
    // Set up selection handling
    auto selection = m_treeview.get_selection();
    selection->signal_changed().connect(sigc::mem_fun(*this, &ContactsWindow::on_selection_changed));
    
    // Set up double-click handling
    auto click_gesture = Gtk::GestureClick::create();
    click_gesture->set_button(GDK_BUTTON_PRIMARY);
    click_gesture->signal_pressed().connect(sigc::mem_fun(*this, &ContactsWindow::on_treeview_pressed));
    m_treeview.add_controller(click_gesture);
    
    // Add the tree view to a scrolled window
    m_scrolled_left.set_child(m_treeview);
    m_scrolled_left.set_hexpand(true);
    m_scrolled_left.set_vexpand(true);
    
    // Create the detail view
    m_detail_box.set_orientation(Gtk::Orientation::VERTICAL);
    m_detail_box.set_spacing(10);
    m_detail_box.set_margin(10);
    
    m_detail_name.set_wrap(true);
    m_detail_name.set_xalign(0);
    m_detail_name.add_css_class("title-1");
    
    m_detail_email.set_wrap(true);
    m_detail_email.set_xalign(0);
    
    m_detail_phone.set_wrap(true);
    m_detail_phone.set_xalign(0);
    
    m_edit_button.set_label("Edit");
    m_edit_button.set_sensitive(false);
    m_edit_button.signal_clicked().connect(sigc::mem_fun(*this, &ContactsWindow::on_edit_clicked));
    
    m_detail_box.append(m_detail_name);
    m_detail_box.append(m_detail_email);
    m_detail_box.append(m_detail_phone);
    m_detail_box.append(m_edit_button);
    
    // Add the detail view to a scrolled window
    m_scrolled_right.set_child(m_detail_box);
    m_scrolled_right.set_hexpand(true);
    m_scrolled_right.set_vexpand(true);
    
    // Add the scrolled windows to the paned container
    m_paned.set_start_child(m_scrolled_left);
    m_paned.set_end_child(m_scrolled_right);
    m_paned.set_position(300);
    
    // Set the paned container as the main window content
    set_child(m_paned);
    
    // Set up application actions
    auto app = dynamic_cast<Gtk::Application*>(get_application());
    app->add_action("about", sigc::mem_fun(*this, &ContactsWindow::on_about));
    app->add_action("quit", sigc::mem_fun(*this, &ContactsWindow::on_quit));
    app->set_accels_for_action("app.quit", {"<Ctrl>q"});
  }
  
protected:
  void on_add_clicked() {
    ContactDialog dialog(*this, "Add Contact");
    int response = dialog.run();
    
    if (response == Gtk::ResponseType::OK) {
      std::string name = dialog.get_name();
      std::string email = dialog.get_email();
      std::string phone = dialog.get_phone();
      
      if (!name.empty()) {
        m_model->add_contact(name, email, phone);
      }
    }
  }
  
  void on_edit_clicked() {
    auto selection = m_treeview.get_selection();
    auto iter = selection->get_selected();
    
    if (iter) {
      int id = (*iter)[m_model->columns().id];
      Glib::ustring name = (*iter)[m_model->columns().name];
      Glib::ustring email = (*iter)[m_model->columns().email];
      Glib::ustring phone = (*iter)[m_model->columns().phone];
      
      ContactDialog dialog(*this, "Edit Contact", name, email, phone);
      int response = dialog.run();
      
      if (response == Gtk::ResponseType::OK) {
        std::string new_name = dialog.get_name();
        std::string new_email = dialog.get_email();
        std::string new_phone = dialog.get_phone();
        
        if (!new_name.empty()) {
          m_model->update_contact(id, new_name, new_email, new_phone);
        }
      }
    }
  }
  
  void on_delete_clicked() {
    auto selection = m_treeview.get_selection();
    auto iter = selection->get_selected();
    
    if (iter) {
      int id = (*iter)[m_model->columns().id];
      Glib::ustring name = (*iter)[m_model->columns().name];
      
      auto dialog = Gtk::AlertDialog::create("Delete Contact?");
      dialog->set_detail("Are you sure you want to delete \"" + name + "\"?");
      dialog->set_buttons({"Cancel", "Delete"});
      dialog->set_cancel_button(0);  // First button (Cancel) is the cancel button
      dialog->set_default_button(1);  // Second button (Delete) is the default button
      
      dialog->choose(*this, [this, id](int response) {
        if (response == 1) {  // Delete button was clicked
          m_model->delete_contact(id);
        }
      });
    }
  }
  
  void on_selection_changed() {
    auto selection = m_treeview.get_selection();
    auto iter = selection->get_selected();
    
    if (iter) {
      Glib::ustring name = (*iter)[m_model->columns().name];
      Glib::ustring email = (*iter)[m_model->columns().email];
      Glib::ustring phone = (*iter)[m_model->columns().phone];
      
      m_detail_name.set_text(name);
      m_detail_email.set_text("Email: " + email);
      m_detail_phone.set_text("Phone: " + phone);
      
      m_delete_button.set_sensitive(true);
      m_edit_button.set_sensitive(true);
    } else {
      m_detail_name.set_text("");
      m_detail_email.set_text("");
      m_detail_phone.set_text("");
      
      m_delete_button.set_sensitive(false);
      m_edit_button.set_sensitive(false);
    }
  }
  
  void on_treeview_pressed(int n_press, double x, double y) {
    if (n_press == 2) {  // Double-click
      auto path_info = m_treeview.get_path_at_position(x, y);
      if (path_info) {
        auto path = std::get<0>(path_info);
        m_treeview.get_selection()->select(path);
        on_edit_clicked();
      }
    }
  }
  
  void on_about() {
    auto dialog = Gtk::AboutDialog::create();
    dialog->set_program_name("Contacts Database");
    dialog->set_version("1.0");
    dialog->set_copyright("Copyright © 2024");
    dialog->set_comments("A simple contact management application");
    dialog->set_authors({"Your Name"});
    dialog->set_license_type(Gtk::License::GPL_3_0);
    
    dialog->set_transient_for(*this);
    dialog->show();
  }
  
  void on_quit() {
    auto app = get_application();
    app->quit();
  }
  
private:
  Database m_db;
  Glib::RefPtr<ContactListModel> m_model;
  
  Gtk::Paned m_paned;
  Gtk::ScrolledWindow m_scrolled_left;
  Gtk::ScrolledWindow m_scrolled_right;
  Gtk::TreeView m_treeview;
  
  Gtk::Box m_detail_box;
  Gtk::Label m_detail_name;
  Gtk::Label m_detail_email;
  Gtk::Label m_detail_phone;
  Gtk::Button m_edit_button;
  
  Gtk::Button m_add_button;
  Gtk::Button m_delete_button;
};

int main(int argc, char* argv[]) {
  auto app = Gtk::Application::create("org.example.contacts");
  return app->make_window_and_run<ContactsWindow>(argc, argv);
}
```

## Troubleshooting

### Common Build Issues

#### Missing Dependencies
```
Package gtk4 was not found in the pkg-config search path.
```
Solution: Install the GTK 4.0 development packages for your system.

#### Version Mismatch
```
Dependency 'gtkmm-4.0' found: NO (required: '>= 4.8.0' found: '4.6.0')
```
Solution: Update your GTK libraries or adjust your required version in the meson.build file.

#### Compiler Errors
```
error: 'class Gtk::Widget' has no member named 'set_child'
```
Solution: This might indicate a method change between GTK versions. Check the API documentation for your specific GTK version.

### Runtime Issues

#### Widget Not Showing
If a widget is not visible:
1. Check if it has been added to a container
2. Ensure all containers and widgets are properly shown with `show()`
3. Check if the widget has a size allocation (set_size_request or expand properties)

#### Signal Handlers Not Working
If signal handlers are not being called:
1. Ensure the connection is stored (for lambda functions)
2. Check for typos in signal names
3. Verify the signal parameters match the handler signature

#### Memory Leaks
1. Use `Glib::RefPtr` for objects that need reference counting
2. Use `Gtk::make_managed()` for widgets added to containers
3. Be careful with circular references in signal handlers

#### Application Crashes
1. Use try-catch blocks to catch and handle exceptions
2. Check for null pointers before dereferencing
3. Use a debugger to trace the issue

### Debugging Tips

#### Logging
Use GLib's logging system for debug messages:
```cpp
g_debug("Debug message: %s", message.c_str());
g_warning("Warning message: %s", message.c_str());
g_error("Error message: %s", message.c_str());  // This will abort the program
```

#### Environment Variables
Set these environment variables for additional debugging:
```bash
# Enable GTK inspector (Ctrl+Shift+D or Ctrl+Shift+I to open)
GTK_DEBUG=interactive

# Show updates as they happen (flashing)
GTK_DEBUG=updates

# Debug layout issues
GTK_DEBUG=layout

# Debug CSS parsing
GTK_DEBUG=css

# Debug memory allocation
G_SLICE=debug-blocks
```

#### GDB with GTK
When using GDB with GTK applications, you might want to ignore some common signals:
```
(gdb) handle SIGPIPE nostop noprint
(gdb) handle SIG32 nostop noprint
(gdb) handle SIG33 nostop noprint
```

## References and Further Reading

### Official Documentation
- [GTK 4.0 API Reference](https://docs.gtk.org/gtk4/)
- [gtkmm 4.0 API Reference](https://developer-old.gnome.org/gtkmm-tutorial/unstable/)
- [GLib API Reference](https://docs.gtk.org/glib/)
- [Cairo Graphics](https://www.cairographics.org/documentation/)

### Books
- "Programming with gtkmm 4" by GNOME Documentation Team
- "GTK/GNOME Application Development" by Havoc Pennington

### Tutorials and Guides
- [GNOME Developer Center](https://developer.gnome.org/)
- [GTK 4 Tutorial](https://toshiocp.github.io/Gtk4-tutorial/)
- [GTK4 Migration Guide](https://docs.gtk.org/gtk4/migrating-3to4.html)

### Example Projects
- [GNOME Builder](https://github.com/GNOME/gnome-builder) - An IDE built with GTK
- [Nautilus](https://github.com/GNOME/nautilus) - The GNOME file manager
- [GTK Demo](https://github.com/GNOME/gtk/tree/main/demos) - Official GTK demos

### Communities and Forums
- [GNOME Discourse](https://discourse.gnome.org/)
- [GTK Matrix Channel](https://matrix.to/#/#gtk:gnome.org)
- [StackOverflow GTK Tag](https://stackoverflow.com/questions/tagged/gtk)

This guide should help you get started with GTK 4.0 development using C++. As GTK continues to evolve, be sure to check the official documentation for the latest information and best practices.