# Natív komponensek

## Új projekt

    npx @react-native-community/cli init NativeModule --template react-native-template-typescript

## Android

Elősször létrehozunk egy RandomView osztályt ami leszármazik az Androidos Button osztályból.

Ehez indítsuk el az Android studiót és válasszuk ki az "Open an existing Android Studio project" lehetőséget és válasszuk ki NativeModule-ban található android mappát.

Ezt követően hozzuk létre a _BulbView.java_ fájlt a következő tartalommal.

```Java
public class RandomView extends Button {
    public RandomView(Context context) {
        super(context);
        this.setTextColor(Color.BLUE);
        this.setText("This button is created from JAVA code");
        this.setOnClickListener(changeStatusListener);
    }

    public RandomView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public RandomView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }
}
```

A következő lépés az, hogy ezt a UI elemet képesek legyünk létrehozni és frissíteni a javascript kódból. Ehhez minden esetben szükség van egy ViewManagerből leszármazó osztályra. Ehhez hozzuk létre a _RandomViewManager.java_-t a következő tartalommal

```Java
public class RandomViewManager extends SimpleViewManager<RandomView> {

    @NonNull
    @Override
    public String getName() {
        return "RandomView";
    }

    @NonNull
    @Override
    protected RandomView createViewInstance(@NonNull ThemedReactContext reactContext) {
        return new RandomView(reactContext);
    }
}
```

Így már van egy olyan osztályunk amely megadja milyen néven érjük el a Natív UI elemünket és képes ebből létrehozni egy példányt. Azonban ezt ebben a formában még nem tudjuk hozzáadni a projekthez. Ahoz még szükség van arra, hogy készítsünk egy ReactPackage osztályt, amely összegyűjti a natív elemeinket és moduljainkat amelyeket szeretnénk a javascript kódban használni. Készítsünk tehát egy _RandomViewPackage.java_ fájlt:

```Java
public class RandomViewPackage implements ReactPackage {

    @NonNull
    @Override
    public List<NativeModule> createNativeModules(@NonNull ReactApplicationContext reactContext) {
        return Collections.emptyList();
    }

    @NonNull
    @Override
    public List<ViewManager> createViewManagers(@NonNull ReactApplicationContext reactContext) {
        return Collections.singletonList(new RandomViewManager());
    }
}
```
