### SwiftUI: Cheat Sheet

#### Udemy course (germany)
[SwiftUI course](https://www.udemy.com/course/swiftui-kurs-mit-ios-14-und-swift-5/)
* costum allert: video 77
* Form: video 81
* Date formatting: video 83

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

Its best practice to make @State a private var (because its only available in its struct). @State varibles can only be value types (no class).

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

#### Group
Use one modifier for multible views.

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
        PassedTo($test)
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

#### Animation
```
withAninamtion(.easeIn(duration: 2.0)) {
    animation.toggle()
}
```
Animates explicitly the affects that are effected by the variables inside the closure. To toggle a value in an affect, use ternary operator:
```
@State saturat = false 
Text("Test").saturation(saturat ? 0.0 : 0.5)
```

#### State object
```
class StateObject: ObservableObject {
    @Published var myProperty = "Hello"
}

struct ViewContent: View {
    @ObservedObject private var stateObject = StateObject()
    
    var body: some View {
        self.stateObject.myProperty = "Hello 123"
    }
}
```

To have a class instance inside our view struct which is changeable inside body var: use @ObservedObject. The class has to implement ObservableObject protocol.
The var of the class, which will be changed inside the body var, has to have the observer @Published (only if the body var should be recomputed on every change of this var).

If you want to decide manually when body is shoud be recomputed, use:
```
let objectWillChange = ObservableObjectPublisher()

var myProperty = "" {
    willSet {
        if condition {
            objectWillChange.send() //sends to view struct: recompute your body var
        }
    }
}
```
There exists an @EnvironmentObject, it omits view chain.

### UI elements
UI elements are views. There have modifier. The order of the modifier inst unimportant. Here I will list them

#### General modifier

* padding() sets space of inner content to outer frame
  * padding(.trailing) choosing padding only one side
  * padding(.horizontal, 50) choosing sides and size
  * padding(10) padding width 10 points
  * padding(EdgeInsets(top: 0, leading: 20, bottom: 0, trailing: 20))
* background(Color.blue) sets background
  * background(View) your can pass a view
  * backgound(RoundedRectangle(cornerRadius: 10))
* frame(width: 100, height: 100, allignment: .center) sets frame
* border(Color.black) used on views
* layoutPriority(1) chooses wihch elements are definitely shown
* shadow(color: Color.black, radius: 10, x: 10, y: 10) makes a shadow
  * shadow(color: Color.black, radius: 10) 

* cornerRadius(10) rounded corners
* stroke(Color.black) makes frame border (use when shape)
  * stroke(Color.black, lineWidth: 4)
* scaleEffect(10) scale view by specified number
  * scaleEffect(x: 1, y: 5) scales in directions
* ignoreSaveArea() fills hole screen 
* clipShape(Circle()) only show what is inside the clip shap
* offset(x: 20, y: 50) offsets view by the amount specified
* animation(.string()) add after effects (they will then be animated)
  * animation(.linear()) there are multible animation arguments
  * for multible animations: it effects effects from buttom to top, starts at the animation call and ends at the next animation call (or end of modifieres) 
* overlay(Text("Text")) The view to layer in front of this view
* rotationEffect(.degree(-90)) rotates view
* opacity(0.5) 0 to 1, 1 = 100% 
* plendMode(.multiply) for compositing a view with overlapping content
* colorMultiply(.blue) mixes color with view
* saturation(0.3) black / white
* contrast(0.6)
* onAppear {} when view appears, the code in the closure will be started
* onDisappear {} when view disappears, the code in the closure will be started
* sheet(isPresented: True, content: {}) presents a swip down view

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
```
Text("Text")
```

Modifier:
* font(.title) (predefined fonts: fit to the current screensize)
  * font(.system(size: 30)) defines a size
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
Button(action: {}, label: {})
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
}.onDelete(perform: { indexSet in 
    self.arrayData.remove(atOffsets: indexSet) // adds delete capabilitie
})
```
Struct should inherit: Identifiable.
```
struct arrayData: Identifiable {
    var id = UUID()
    let data = "data"
}
```
You can delete capabilitie:
```
ForEach(data, id: \.id) { dataElem in
    //you views
}.onDelete(perform: { indexSet in
    self.data.remove(atOffsets: indexSet)
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
* ListStyle(PlainListStyle()) defines list style

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
You can have add a range:
```
let fromToday = Calender.current.date(byAdding: .minute, value: -1, to: Date())!
DatePicker("", selection: $date, in: fromToday...) //time
```

To format the date: watch video 83 (link in the tutorial beginning)
Udemy course (germany) [Udemy Course](#udemy-course-germany)
Modifier:
* .datePickerStyle(GraphicalDatePickerStyle()) makes always on selector

#### Slider
```
@State var number = 0.0
Slider(value: $number, in 1...100, step 1, onEditingChanged: { _ in }) 
Slider(value: $number) 
```

Number formatting: end of video 84 [Udemy Course](#udemy-course-germany)

Modifier:
* accentColor(.orange) makes color of the slider it self

#### Stepper
```
Stepper("Stepper", value: $stepperValuje)
```
Simple stepper.
```
Stepper("Stepper") {
    //pressed +
} onDecrement: {
    //pressed -
} onEditingChanged: { (bool) in 
    //when one of the button was pressed
} 

Stepper("Stepper", value: $stepperValuje, in: 1...5)
```

#### Picker
```
Picker(selection: $selectedItem, label: Text(""), content: {
    Text("1").tag(1)
    Text("1").tag(1)
})
```
Value in the tag(1) modifier will be stored in $selectedItem. <br>
With array:
```
let coutries = ["Germany, "USA"]
Picker(selection: $selectedItem, label: Text(""), content: {
    ForEach(0..<countries.count) {
        Text(countries[$0])
    }
})
```
$selectedItem will be filled with the index.

Modifier:
* pickerStyle(SegmentedPickerStyle())

#### Scroll view
```
ScrollView() {
    //A lot of vertical views
}
ScrollView(.horizontal, showsIndicators: false) {
    //A lot of horizontal views
}
```
showsIndicators can be true or false (defalut = true).


