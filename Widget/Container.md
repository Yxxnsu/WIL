# [Widget] Container

Tag: Widget
속성: 완료

# ✔️ Container Opt

Container는 Child widget의 Size를 따른다. 만약 Child가 없을 땐 확장됨.

## Width & Height

- DES

    Container의 크기를 임의로 지정해준다.

    ```dart
    Container(
    	width: 200,
      height: 200,
    )
    ```

## Margin & Padding

- DES

    Container의 여백들을 설정해준다.

    ```dart
    Container(
    	padding : EdgeInset.all(3), //padding은 child가 있을 때 적용
      margin: EdgeInset.all(4),
    )

    //margin은 Container 외부의 여백을 조정해주고
    //padding은 Container 내부의 여백을 조정한다.
    ```

## Alignment

- DES

    Container의 크기가 Child의 크기보다 클 때 즉, 내부에 child가 움직일 공간이 있을 때 사용할 수 있다. Child의 위치를 Container 내부에서 조정할 수 있다. 

    ```dart
    Container(
    	alignmnet: Alignment.center,
    	child:Text('Yxxnsu');
    )
    ```

## Decoration

- DES

    Container의 Style을 바꿀 수 있다.

    ```dart
    Container(
    	decoration: BoxDecoration(
    		1. color: Colors.Red => Container 색 지정
    		2. border: Border.all(color,width,style) => 테두리 Style 지정
    		3. borderRadius: BorderRadius.all(Radius.circular(30)) => 테두리를 곡선으로 만들 수 있다.
    		4. shape: BoxShape.circle & Rectangle => Container 모양 변경
    	)

    ```

## Transform

- DES

    Container의 기울기와 위치 등을 설정할 수 있다.

    ```dart
    Container(
    	transform: Matrix4.rotationZ(0.4);
    )
    ```