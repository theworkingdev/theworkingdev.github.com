---
title: A basic ListView in Android
layout: post
categories: android
permalink: /2012/10/a-basic-listview-in-android.html
date:   2012-10-05 
---

Source files at the [github page](https://www.workingdev.net/2012/10/git@github.com:tedhagos/BroadcastReceiverManifest.git)

What we want to do;

* Know the basic step needed to display an array into a listview
* Create a new layout to be used by the listview
* Use an Adapter to connect an array of values to the listview

First, **Create a project with an empty activity**. Name the project ListViewSample. Add a ListView to the main activity layout.

![](/images/listview-lab.png)

Next, Create a new layout.

You can do this in Android Studio via File > New > XML> Layout XML File. You will be asked for the layout file name and the root tag. Use the following values

Layout File Name : myitem
Root Tag: TextView
We created this new layout because this will effectively be a single row in our listview. We are displaying only one item of data per row, hence, a simple textview

Next, Implement the onCreate method of the main activity.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_main);
  populateList(); // we'll create this later
  handleEvents(); // we'll create this later
} 
```

After that, we should create the populateList method

```java
private void populateList() {
  // The Array
  String[] numbers = {"One", "Two", "Three", "Four"};
  
    // The Adapter
  ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,R.layout.myitem, numbers);

  // The ListView
  ListView listview = (ListView) findViewById(R.id.listView);
  listview.setAdapter(adapter);
}
```

Then, let's implement the handleEvents method

```java
private void handleEvents() {
  ListView listview = (ListView) findViewById(R.id.listView);
  assert listview != null;

  listview.setOnItemClickListener(new AdapterView.OnItemClickListener() {
      
    @Override
    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
      String item_name = ((TextView) view).getText().toString();
      String message = String.format("You clicked %s : %s", position, item_name);
      Toast.makeText(getBaseContext(), message, Toast.LENGTH_LONG).show();
    }
  });
}
```

 