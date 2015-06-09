To customize theme of `SnappingActivity` you need to set `ThemesProvider` in `ScanbotSDKInitializer`:

    new ScanbotSDKInitializer()
            .themesProvider(new ThemesProvider() {
                @Override
                public int getThemeResId(String s) {
                    return R.style.CustomScanbotTheme;  // Your Theme
                }
            })
            .initialize(this);

Your Theme must inherit from `ScanbotTheme`:

    <!-- See ScanbotTheme for full list of attributes -->
    <style name="CustomScanbotTheme" parent="ScanbotTheme">
        <item name="main_color">@color/material_deep_teal_500</item>
    </style>