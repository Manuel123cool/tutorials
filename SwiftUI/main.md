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
SwiftUI doesnt know how to arrange your view, so there are containers.

### Containers
You can modify the stack: and every child element will inherit these 
modifier.

These containers can have a argument spacing: which defines the space between the cild elements.
```
VStack(spacing: 20)
```

There can have the argument: alignment. These align the inner connent:
* leading (right side)
* trailing (left side)
* center

#### VStack
Arranges the elements vertical.

#### HStack
Arranges the elements horizontal.

### UI elements
UI elements are views. There have modifier. Here I will list them

#### General modifier

* padding() sets space of inner content to outer frame
* background(Color.blue) sets backgound
  * background(View) your can pass a views
  * backgound(RoundedRectangle(cornerRadius: 10))
#### Text

Text("Text")

Modifier:
* font(.title) (predefined fonts: fit to the current screensize)
  * font(.system(syize: 30)) defines a size
  * font(.costum("Arial", size: 30))
* foregroundColor(Color.white) defines a fontstyle
* bold() 

#### Image

Image("Test")

Arguments:
* Image(systemName: "arrow.left")
Modifier:

