### I am currently learning swiftUI, for now: this are some notes


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

SwiftUI doesnt know how to arrange your view, so there are containers.

#### Containers
You can modify the stack: and every child element will inherit these 
modifier.
##### VStack
Arranges the elements vertical.

##### HStack
Arranges the elements horizontal.

##### UI elements
UI elements are views. There have modifier. Here I will list them

##### Text
```
Text("Text")

Modifier:
* font(.title) (predefined fonts: fit to the current screensize)
* bold() 
