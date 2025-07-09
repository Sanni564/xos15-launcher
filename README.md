# xos15-launcher
Android launcher inspired by XOS 15
private GridView gridView;
private List<ResolveInfo> apps;
private PackageManager packageManager;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    packageManager = getPackageManager();
    gridView = findViewById(R.id.gridView);

    loadApps();

    gridView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
        @Override
        public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
            ResolveInfo app = apps.get(position);
            Intent launchIntent = packageManager.getLaunchIntentForPackage(app.activityInfo.packageName);
            if (launchIntent != null) {
                startActivity(launchIntent);
            } else {
                Toast.makeText(MainActivity.this, "Cannot open this app.", Toast.LENGTH_SHORT).show();
            }
        }
    });
}

private void loadApps() {
    Intent intent = new Intent(Intent.ACTION_MAIN, null);
    intent.addCategory(Intent.CATEGORY_LAUNCHER);

    apps = packageManager.queryIntentActivities(intent, 0);
    AppAdapter adapter = new AppAdapter(this, apps);
    gridView.setAdapter(adapter);
}
