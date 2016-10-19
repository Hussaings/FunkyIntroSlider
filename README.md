# FunkyIntroSlider
Adding Intro slider Reflects the things an app can do and what an app contains.
FunkyIntroSlider provides you a simple way to add an intro slider to your app. 

Intermediate Slides           |  Last Slide
:-------------------------:|:-------------------------:
![](https://github.com/Hussaings/FunkyIntroSlider/blob/master/Screenshot_2016-10-19-09-44-28.png)  |  ![](https://github.com/Hussaings/FunkyIntroSlider/blob/master/Screenshot_2016-10-19-09-44-38.png)
-----------------------------------------------------------------------------------------------------------------------

##Download [FunkyIntroSlider.jar](https://github.com/Hussaings/FunkyIntroSlider/blob/master/FunkyIntroSlider.jar)
add jar to app/libs/ 

## Add Dependency 
add following lines in build.gradle (app module)
```
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile files('libs/FunkyIntroSlider.jar')
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
get slides from above repository or design your own layouts
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
##Adding Dot Indicator

 Call another version of Constructor of Reference Class
 ```
 Reference reference=new Reference(viewPager,btnNext,btnlast,dotsLayout,R.drawable.selecteditem_dot,R.drawable.nonselecteditem_dot);
```
dotsLayout(LinearLayout)
and call a static method
```
AddBottomDots.addDots(getBaseContext(),0,reference);
```

##Parallaxing effects
for adding parallaxing effects,Create a java class MypageTransformer
Add below code to MypageTransformer.java
```
//you can extends any of the 4 transformer listed above
public class MypageTranformer extends SlidePageTransformer {
    @Override
    public void transformPage(View page, float position) {
        super.transformPage(page, position);
        int pagePosition = (int) page.getTag();
        int pageWidth = page.getWidth();
        int pageHeight = page.getHeight();
        float pageWidthTimesPosition = pageWidth * position;
        float absPosition = Math.abs(position);
        
        //getting views and adding animations
        View title = page.findViewById(R.id.title);
        title.setAlpha(1.0f - absPosition);
        
        //animation on description
        View description = page.findViewById(R.id.description);
        description.setTranslationY(-pageWidthTimesPosition / 2f);
        description.setAlpha(1.0f - absPosition);

        View Image = page.findViewById(R.id.image);

        if (pagePosition >= 0 && Image != null) {
            Image.setAlpha(1.0f - absPosition);
            Image.setTranslationX(-pageWidthTimesPosition * 1.5f);

        }
    }
}
```
Now call pageTransformer in MainActivity as
```
viewPager.setPageTransformer(false, new MypageTranformer());
```
<img src="https://github.com/Hussaings/FunkyIntroSlider/blob/master/Parallaxing.gif" width="300">

This is Simple parallaxing effects,you can set your own animation in MypageTransformer.java file.
