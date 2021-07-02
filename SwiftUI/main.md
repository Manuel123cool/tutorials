### SwiftUI: Quick and Dirty


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

### UI elements
UI elements are views. There have modifier. Here I will list them

#### General modifier

* padding() sets space of inner content to outer frame
  * padding(.trailing) choosing padding only one side
  * padding(10) padding width 10 points
  * padding(EdgeInsets(top: 0, leading: 20, buttom: 0, trailing: 20))
* background(Color.blue) sets backgound
  * background(View) your can pass a views
  * backgound(RoundedRectangle(cornerRadius: 10))
* frame(width: 100, height: 100, allignment: .center) sets frame
* border(Color.black) 
* layoutPriority(1) chooses wihch elements are definitely shown
* shadow(color: Color.black, radius: 10, x: 10, y: 10) makes a shadow
* cornerRadius(10) rounded corners
* stroke(Color.blac)

#### Text
```
Text("Text")
```

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
  * take hole space and ratio stays same (cuts of image)
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
This uses as much as space as possible, with multible spacers, it shares the space (each get as much as possible).

#### Color
```
Color.init(red: 135 / 255, green: 233 / 255, blue: 22 / 255)
```
Color.init(red: 135 / 255, green: 233 / 255, blue: 22 / 255)
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
Modifier:
* stroke(Color.black, lineWidth: 2) makes a border

#### Textfield
```
TextField("Placeholder", text: $bindedText, onEditingChanged: { (returnType) in }, onCommit: {})
```
onEditingChanged shows if the user is in the textfield. onCommit shows if user changes the text. onCommit runs if user pressed return.

Modifier:
* textFieldStyle(RoundedBorderTextFieldStyle())
* foregroundColor(.blue) chagens the inserted text to a certain color.

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


Hides entered text. Much is same with TextField. In onCommit function you can get the password with: self.password

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
