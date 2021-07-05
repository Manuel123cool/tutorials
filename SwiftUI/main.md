### SwiftUI: Quick and Dirty

#### Udemy course 
[SwiftUI course](https://www.udemy.com/course/swiftui-kurs-mit-ios-14-und-swift-5/)
* costum allert: video 77
* Form: video 81

#### Everything is a view

The structure of them is as followed:
```
struct NewView: View {
    @State var stateVar: String = ""

    var body: some View {
        //View content
    }
}
```

Variables that can be changed by the UI elements, are marked width: 
```
@State
```
Body is a computed property, if a @State var changes, it will comput its content again. So the code inside it will be runed again.

SwiftUI doesnt know how to arrange your view, so there are containers.

### Containers
Dont get confused: the container use a spacial syntax. Basically, the block after The struct VStack {} is a VStack(content: {}) function argument.

You can modify the stack: and every child element will inherit these 
modifier.

These containers can have a argument spacing: which defines the space between the cild elements.
```
VStack(spacing: 20) {}
```

There can have the argument: alignment. These align the inner connent:
* leading (right side)
* trailing (left side)
* center

```
VStack(alignment: .top) {}
```

#### VStack
Arranges the elements vertical.

#### HStack
Arranges the elements horizontal.

#### ZStack
Arranges elements on top of each other. Has more elignment possibilities.
Like topTriailing (top left).

#### GridLayout
Arranges elements in a grid. 

#### Windows preview
```
struct Navigation_Preview: PreviewProvider {
    struct var previews: some View {
        WindowView1()
        WindowView2()
    }
}
```

#### App start view
```
import SwiftUI

@main
struct UITutorialApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView() //this is the start view 
            // (replace with your view)
        }
    }
}

```

#### Reuse or organize code
```
struct ReUse: View {
    var body: some View {
        VStack {
            Text("1")
            Text("2)
        }
    }
}
//of course in a other body variable
HStack {
    ReUse()
    ReUse()
}
```

#### Binding
If you want to pass a state var to another struct, use @Binding in the inner struct where the state var is passed to. 
```
struct IsPassing: View {
    @State var test = ""
    var body: some View {
        PassedTo(test)
    }
}

struct PassedTo: View {
    @Binding var test: String
    var body: some View {
        Text(test)
    }
}
```
If you want to fake bind (to test something) use:
```
.constant(true)
```

#### Getting screen size
```
let sreenSize = UIScreen.main.bounds
```
With width, height you get the sizes.

### UI elements
UI elements are views. There have modifier. Here I will list them

#### General modifier

* padding() sets space of inner content to outer frame
  * padding(.trailing) choosing padding only one side
  * padding(.horizontal, 50) choosing sides and size
  * padding(10) padding width 10 points
  * padding(EdgeInsets(top: 0, leading: 20, buttom: 0, trailing: 20))
* background(Color.blue) sets background
  * background(View) your can pass a view
  * backgound(RoundedRectangle(cornerRadius: 10))
* frame(width: 100, height: 100, allignment: .center) sets frame
* border(Color.black) 
* layoutPriority(1) chooses wihch elements are definitely shown
* shadow(color: Color.black, radius: 10, x: 10, y: 10) makes a shadow
* cornerRadius(10) rounded corners
* stroke(Color.blac)
* scaleEffect(10) scale view by 
* ignoreSaveArea() fills hole screen 
* clipShape(Circle()) only show what is inside the clip shap
* offset(x: 20, y: 50) offsets view by the amount specified
* animation(.string()) for not I dont know how its work

##### Sheet
```
sheet(isPresented: $showSheet, content: { View() }) 
```

Content is what is shown when clicked, when $showSheet is true. You can give you constum view a paramter which is void function type and give it a function which makes showSheet false (to go back) and call this function in the constum view.

#### Gesture regognizer
```
View.gesture(
    TapGesture()
        .onEnded({
            // action when tap ended
        })
)
```

TapGesture can take argument count: how many times the view is clicked until the function runs.

```
let longPressGesture = longPressGesture()
    .onEnded({ _ in
        name = "long pressed"
    })

```
Function runs when long pressed.

#### Text

Text("Text")


Modifier:
* font(.title) (predefined fonts: fit to the current screensize)
  * font(.system(syize: 30)) defines a size
  * font(.costum("Arial", size: 30))
* foregroundColor(Color.white) defines a fontstyle
* bold() 
* fontWeight(.light) makes font lighter or bolder
* italic()
* lineLimit(1) limit text to certain line length
  * lineLimit(nil) shows text always
* multilineTextAlignment(.leading) aligns text
* baseLineOffset(30) changes baseline verticcal
* kernig(5.0) makes space between letters
* textCase(.upperCase) makes text lovercase or uppercase
* underline(true, Color.red) make line under the txt

* clipShape(Capsule()) like background with shape

#### Image
```
Image("Test")
```
Image argument: the image name.

Modifier:
* Image(systemName: "arrow.left") handles as text (same modifier)
Modifier:
* clipeShape(Cricle()) makes a shape background, works good with padding
* rezisible() adjusts image to given size
* aspectRatio(contentMode: .fit) ratio stays same
  * aspectRatio(contentMode: .fill) take hole space and ratio stays same (cuts of image)
#### Button
```
Button(action {}, label: {})
```

Action is what happens, when button is pressed. Label is how it looks.

#### Rectangle
```
Rectangle()
```

Modifier:
* foregroundColor(Color.white) defines the color

#### Spacer
```
Spacer()
```
This uses as much as space as possible, with multible spacers, it shares the space (each gets as much as possible).

#### Color
```
Color.init(red: 135 / 255, green: 233 / 255, blue: 22 / 255)
```
Divide rgb color width 255.

Modifier:
* edgesIgnoringSafeArea(.all) filles button and top fully
* opacity(0.2) adjusts brightness

#### Divider
```
Divider()
```
Makes a line. Shows something is divided.

Modifier:
* background(Color.grenn)
* frame(width: 100) make divider smaller

#### Rounded Rectangle
```
RoundedRectangle(cornerRadius: 10)
```
Arguments:
```
RoundedRectangle(cornerRadius: 10, style: .continuous)
```
Modifier:
* stroke(Color.black, lineWidth: 2) makes a border

#### Textfield
```
TextField("Placeholder", text: $bindedText, onEditingChanged: { (returnType) in }, onCommit: {})
```
onEditingChanged shows if the user is in the textfield. onCommit runs if user pressed return.

Modifier:
* textFieldStyle(RoundedBorderTextFieldStyle())
* foregroundColor(.blue) changes the inserted text to a certain color.
* disableAutocorrection(true)
* keyboardType(.numberPad)

#### Secure textfield
```
SecureField("Placeholder", text: $password, onCommit: {})
```
Hides entered text. Much is same with TextField. In onCommit function you can get the password with: self.password

#### Toggle
```
Toggle(isOn: $myState, label: {})
```
isOn binds variable to true or false. label function makes the style.

#### Label
```
Label( title: {}, icon: {})
```
same as:
```
HStack {
    Text("Hallo Bello")
    Image("person")
}
```

#### Table
```
List {
    Text("Row one")
    Text("Row two")
}
```
This makes simple rows.
You can use it to list array data.
```
List(array, id: \.id) { arrayElem in
    //itarates throw array of structs
    // here you can put each row which eccesses arrayElem
}
```
Struct should inherit: Identifiable.
```
struct arrayData: Identifiable {
    var id = UUID()
    let data = "data"
}
```
You can add delete capability.
```
onDelete(perform: { indexSet in 
    self.arrayData.remove(atOffsets: indexSet})
})
```
Array should be @State, as it can be changed (when added delete), in the body property.

You can add sections.
```
Section(header: Text("Section 1") {
    //Code for drawing rows
    Text("Row one")
    Text("Row two")
}
```
List takes each view as one row, you can combined views into container for multible views into one row.

You can make with navigation: a destination page, for seeing (for example) more details of the row element.

Modifier:
* listRowBackground(View)
  * listRowBackground(Color.green)

#### Navigation
```
NavigationView {
    NavigationLink(destination: DestinationView()) {
        LinkView()
    }
    .navigationBarTitle("Title")
    .navigationBarItems(trailing: View)
}
```
NavigationView is the current view. Or this view you get back when returning. NaviationLink specifies destination and style of a link. naviagationTitle makes a title for the current view and a link name to get back from the destination view. navigationBarItems add a view into the navigation bar.

navigationBarTitle can take a second argument: 
```
.navigationBarTitle("Title", displayMode: .large) 
// .large isnt the only opition
```

You can give destination view arguments, for transfering data.

Modifier for NavigationView:
* accentColor(.black) sets color of get back link

#### Tabview
```
TabView(selection: .constant(1), content: {
    Text("Tab content 1").tabItem {Text("Tap label 1")}.tag(1)
    Text("Tab content 2").tabItem {Text("Tap label 2)}.tab(2)
}
```
Your should make a selection state variable:
```
@State var selesction = 0
```
For storing the selesction.

tag is ther for changing manuelly the content. So if you have a button:
```
Button(action: {
    self.selection = 2
}, label: {})
```
When you press it you get from TabView the conent where tag(2).

#### Alertbox
```
@State var showingAlert = false

Button(action: {
    showingAlert = true
}) {
//view
}
.alert(isPresented: $showingAlert, content: {
    Alert(title:Text("Achtung"), message: Text("Test message"),
        dismissButton: .default(Text("Cancel")))
}
```
message can be nil
If alert is runed it makes showingAlert false.

It exists a nother initializert for Alert.
```
Alert(title:Text("Achtung"), message: Text("Test message"),
    primaryButton .destructive(Text("delete") {
    //happens when pressing primary button
    }, secondaryButton:
        default(Text("test"))
```
destructive() makes a delete style

#### Action sheet
```
@State var showingActionSheet = false

Button(action: {
    showingActionSheet = true
}) {
//view
}
.actionSheet(isPresented: $showingAlert, content: {
    ActionSheet(title: Text("Actions"), message: nil, buttons: 
        [ 
            .deffault(Text("Button1")) {
                Text("Action when press button1")
            }
            .deffault(Text("Button2"))
            .deffault(Text("Button3"))
        ])
}
```
#### Datepicker
```
@State var date = Date()

DatePicker("Datepicker", selection: $date)
```
This initializer picks date and time.
To select only one of them us:
```
DatePicker("Datepicker", selection: $date, displayedComponents: .date) //date
DatePicker("Datepicker", selection: $date, displayedComponents: .hourAndMinute) //time

```
To select only one of them us:
