# FunkyIntroSlider
Adding Intro slider Reflects the things an app can do and what an app contains.
FunkyIntroSlider provides you a simple way to add an intro slider to your app. 

Intermediate Slides           |  Last Slide
:-------------------------:|:-------------------------:
![](https://github.com/Hussaings/FunkyIntroSlider/blob/master/Screenshot_2016-10-19-09-44-28.png)  |  ![](https://github.com/Hussaings/FunkyIntroSlider/blob/master/Screenshot_2016-10-19-09-44-38.png)
-----------------------------------------------------------------------------------------------------------------------

##Download [IntroSlider.jar](https://github.com/Hussaings/FunkyIntroSlider.git)
add jar to app/libs/ 

## Add Dependency 
add following lines in build.gradle (app module)
```
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile files('libs/introslider.jar')
}
```
get Resources from [res](https://github.com/Hussaings/FunkyIntroSlider/tree/master/res)

##Include
setContentView(R.layout.SliderScreen) of Launcher activity 
[SliderScreen.xml](https://github.com/Hussaings/FunkyIntroSlider/blob/master/SliderScreen.xml)

In AndroidManifest.xml add
```
android:theme="@style/Theme.AppCompat.NoActionBar"
```
to Launcher Activity

##Getting Id's
```
viewPager = (ViewPager) findViewById(R.id.view_pager);
        dotsLayout = (LinearLayout) findViewById(R.id.layoutDots);
        btnNext = (ImageButton) findViewById(R.id.btn_next);
        btnlast = (ImageButton) findViewById(R.id.btn_last);
```
##Adding Slides
```
AddSlide addslide=new AddSlide(4); //pass No.of slides as argument(max:6)

        addslide.set(R.layout.slide1, 0);
        addslide.set(R.layout.slide2, 1);
        addslide.set(R.layout.slide3, 2);
        addslide.set(R.layout.slide4, 3);
```
##Add References
```
Reference reference=new Reference(viewPager,btnNext,btnlast);
```
##Adding to MainActivity.java
Your MainActivity Should be Launcher Activity.
```
 // Making notification bar transparent
        if (Build.VERSION.SDK_INT >= 21) {
            getWindow().getDecorView().setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_STABLE |
                    View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN);
        }
```
Changing Notification Bar Color
```
        ChangeStatusBarColor.changeStatusBarColor(MainActivity2.this);
```
Setting Listeners to Viewpager
```
//add Adapter to viewpager
viewPager.setAdapter(new MyViewPagerAdapter(getBaseConte()));

//add pageTransformer to viewpager
viewPager.setPageTransformer(false, new SlidePageTransformer());   //SimpleSlide Effect
```
Add these Lines To change the Sliding Effects
```
viewPager.setPageTransformer(false, new ILLusionPageTransformer()); //Illusioin Effect
viewPager.setPageTransformer(false, new SlidePageTransformer());   //ZoomInOut Effect
viewPager.setPageTransformer(false, new DepthPageTransformer());   //DepthPaging Effect
```

```
//add pageChangeListener to viewpager
viewPager.addOnPageChangeListener(new PageChangeListener(getBaseContext(),reference));
```
##Adding Listener to Next and Check Button
```
btnNext.setOnClickListener(new ButtonNextListener(getBaseContext(),reference));
        
 //Check button       
btnlast.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                startActivity(new Intent(MainActivity.this, MainActivity2.class));
                //MainActivity2 is your Next Activity After Slider is exited
                finish();
            }
        });
        
 ```
 
 ###Adding Dot Indicator
 Call another version of Constructor of Reference Class
 ```
 Reference reference=new Reference(viewPager,btnNext,btnlast,dotsLayout,R.drawable.selecteditem_dot,R.drawable.nonselecteditem_dot);
```
dotsLayout(LinearLayout)
and call a static method
```
AddBottomDots.addDots(getBaseContext(),0,reference);
```
