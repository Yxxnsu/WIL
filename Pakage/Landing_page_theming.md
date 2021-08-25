# Make Landing Page

# ✔️ Landing Page

- 수많은 Component로 이루어진 Lading page

    ![Make%20Landing%20Page%20820ded7b58114dca85fdb27805d760bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.38.30.png](Make%20Landing%20Page%20820ded7b58114dca85fdb27805d760bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_4.38.30.png)

# ✔️ 목표

## 1. Responsive

## 2. Easy reuse

## 3. As similar as possible

# ✔️ Basic Setting

### 1. Color Theme Setting

✨ Landing Page Component 에서 사용되는 색상들을 Class 화 시켜서 정의해줬습니다.

```dart
class AppColors{

  //Gray
  static const kGray900 = Color(0xFF18191F);
  static const kGray800 = Color(0xFF474A57);
  static const kGray700 = Color(0xFF969BAB);
  static const kGray300 = Color(0xFFD9DBE1);
  static const kGray200 = Color(0xFFEEEFF4);
  static const kGray100 = Color(0xFFF4F5F7);

  //Purple
  static const kPurple = Color(0xFF8C30F5);
  static const kPurple800 = Color(0xFFD6B1FF);
  static const kPurple100 = Color(0xFFF1E4FF);

  //Turquise
  static const kTurquise = Color(0xFF2EC5CE);
  static const kTurquise800 = Color(0xFF75E3EA);
  static const kTurquise100 = Color(0xFFD5FAFC);

  //Oragne
  static const kOrange = Color(0xFFFE9A22);
  static const kOrange800 = Color(0xFFFFC278);
  static const kOrange100 = Color(0xFFFFE3C1);

  //Pink
  static const kPink = Color(0xFFF22BB2);
  static const kPink800 = Color(0xFFFF72D2);
  static const kPink100 = Color(0xFFFFB1E6);

  //ETC.
  static const kPastelGreen = Color(0xFFC1E5C0);
  static const kPastelBlue = Color(0xFFC0DAE5);
  static const kPeach = Color(0xFFF39F9F);
  static const kLightPeach = Color(0xFFFDD9D9);
  static const kCandy = Color(0xFFFFC3D8);
  static const kCyan = Color(0xFFA0DCFF);

}
```

### 2. TextStyle Setting

✨ 쓰이는 모든 TextStyle을 따로 file을 만들어서 정의해놓고 사용했습니다.

```dart
TextStyle headLine1({Color color = AppColors.kGray900}) => TextStyle(
    fontSize: fontResponsive(72),
    color: color,
    fontWeight: FontWeight.w900,
    height: 98 / 72);

TextStyle headLine2({Color color = AppColors.kGray900}) => TextStyle(
    fontSize: fontResponsive(48),
    color: color,
    fontWeight: FontWeight.w800,
    height: 64 / 48);

TextStyle headLine3({Color color = AppColors.kGray900}) => TextStyle(
    fontSize: fontResponsive(40),
    color: color,
    fontWeight: FontWeight.w800,
    height: 54 / 40);

TextStyle headLine4({Color color = AppColors.kGray900}) => TextStyle(
    fontSize: fontResponsive(28),
    color: color,
    fontWeight: FontWeight.w800,
    height: 40 / 28);
```

### 3. App Theme

- Button Theme
- Font

```dart
class AppTheme{
  AppTheme._();

  static final ThemeData landingTheme = ThemeData.light().copyWith(
    textTheme: GoogleFonts.interTextTheme(),
    elevatedButtonTheme: ElevatedButtonThemeData(
      style: ElevatedButton.styleFrom(
        onPrimary: null,
        elevation: 0,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(sizeResponsive(8)),          
        ),        
      ),
    ),
    textButtonTheme: TextButtonThemeData(      
       style: ButtonStyle(        
         overlayColor: MaterialStateProperty.all(
           Colors.transparent,
         ),
       )
    ),
    primaryColor: AppColors.kPurple,
    scaffoldBackgroundColor: Colors.white
  );
}
```

### 4. Responsive.dart

✨ 각 Component의 웹 , 모바일에 해당하는 위젯을 받아와서 LayoutBuilder를 통해서 설정한 사이즈에 맞게 리턴해주는 Class 입니다.

```dart
class Responsive extends StatelessWidget {
  final Widget? mobile;
  final Widget desktop;

  const Responsive({Key? key, required this.desktop, this.mobile,})
      : super(key: key);

  static bool isMobile() =>
      ScreenUtil().screenWidth < 700;

  static bool isTablet() =>
      ScreenUtil().screenWidth >= 700 &&
      ScreenUtil().screenWidth < 1200 * c;

  static bool isDesktop() => ScreenUtil().screenWidth >= 1200 * c;

  @override
  Widget build(BuildContext context) {
    return LayoutBuilder(
      builder: (context, constraints) {
        if (constraints.maxWidth >= 700) {
          return desktop;
        } else {
          return mobile ?? desktop;
        }
      },
    );
  }
}
```

### 5. 자주 쓰이는 부분들 위젯화

![Make%20Landing%20Page%20820ded7b58114dca85fdb27805d760bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.44.08.png](Make%20Landing%20Page%20820ded7b58114dca85fdb27805d760bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.44.08.png)

# ✔️ Web & Mobile

- Ex. Header

```dart

class Header3 extends StatelessWidget {
  final List<String> textList;

  const Header3({
    Key? key,
    this.textList = contentListHeader3,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {       
    return Responsive(                      <==
      desktop: _web(),                      <==
      mobile: _mobile(),                    <==
    );
  }
```

![Make%20Landing%20Page%20820ded7b58114dca85fdb27805d760bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.47.19.png](Make%20Landing%20Page%20820ded7b58114dca85fdb27805d760bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.47.19.png)

![Make%20Landing%20Page%20820ded7b58114dca85fdb27805d760bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.48.21.png](Make%20Landing%20Page%20820ded7b58114dca85fdb27805d760bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.48.21.png)

# ✔️ Pakage

## 1. GetX : 상태관리

- EX) DropDownButton , PageController ..

## 2. ScreenUtil

## 3. Url Launcher

## 4. flutter_staggered_grid_view

![Make%20Landing%20Page%20820ded7b58114dca85fdb27805d760bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.06.29.png](Make%20Landing%20Page%20820ded7b58114dca85fdb27805d760bf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-08-08_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.06.29.png)

# ✔️ 시연영상

- 감사합니다.