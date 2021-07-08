# [Widget] Button

Tag: Widget
속성: 완료

## Elevated Button (Code)

- Code

    ```dart
    ElevatedButton.icon(
    	icon: Icon(Icons.add),
    	label: Text('hello'),
    	onPressed: () => {},
    	style : 1. ButtonStyle() 
    					2.ElevatedButton.stylefrom()
    	
    	## Radius (StyleForm 사용하는게 이지)
    	style: ButtonStyle(
    	  shape: MaterialStateProperty.all<RoundedRectangleBorder>(
    	    RoundedRectangleBorder(
    	      borderRadius: BorderRadius.circular(18.0),
    	      side: BorderSide(color: Colors.red)
    	    )
    	  )
    	)

    style: ElevatedButton.styleFrom(
        shape: RoundedRectangleBorder(borderRadius: BorderRadius.all(Radius.circular(30))),
    )
    ```

## Outlined Button (Code)

- Code

    ```dart
    style: OutlinedButton.styleFrom(
        shape: RoundedRectangleBorder(borderRadius: BorderRadius.all(Radius.circular(30))),
    )
    ```

## Toggle Button

- Code

    ```dart
    final isSelected = <bool>[false, false];

    ButtonBar(
      children: <Widget>[
         ToggleButtons(
            color: Colors.black.withOpacity(0.60),
            selectedColor: Color(0xFF6200EE),
            selectedBorderColor: Color(0xFF6200EE),
            fillColor: Color(0xFF6200EE).withOpacity(0.08),
            splashColor: Color(0xFF6200EE).withOpacity(0.12),
            hoverColor: Color(0xFF6200EE).withOpacity(0.04),
            borderRadius: BorderRadius.circular(4.0),
            constraints: BoxConstraints(minHeight: 36.0),
            isSelected: isSelected,
            onPressed: (index) {
            //Respond to button selection
              setState(() {
                  for (int i = 0; i < isSelected.length; i++) {
                     isSelected[i] = i == index;
                   }
              });
            },
            children: [
              Icon(Icons.list),
              Icon(Icons.grid_view),
            ],
          ),
       ],
    ),
    ```

## Floating Action Button (Code)

- Code

    ```dart
    floatingActionButton: FloatingActionButton(
       onPressed: _incrementCounter,
       tooltip: 'Increment',
       child: Icon(Icons.add),
    ), 
    ```

## Text Button (=Flat Button)