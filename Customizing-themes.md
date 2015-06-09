To customize theme of `SnappingActivity` you need to set `ThemesProvider` in `ScanbotSDKInitializer`:

    new ScanbotSDKInitializer()
            .themesProvider(new ThemesProvider() {
                @Override
                public int getThemeResId(String s) {
                    return R.style.CustomScanbotTheme;  // Your Theme goes here
                }
            })
            .initialize(this);

